# Manual Completo de C# para Unity
## Do Básico ao Avançado

---

## Sumário

1. [Introdução ao C# e Unity](#1-introdução-ao-c-e-unity)
2. [Fundamentos da Linguagem C#](#2-fundamentos-da-linguagem-c)
3. [Programação Orientada a Objetos](#3-programação-orientada-a-objetos)
4. [Unity Essentials](#4-unity-essentials)
5. [Scripts em Unity - Nível Básico](#5-scripts-em-unity---nível-básico)
6. [Scripts em Unity - Nível Intermediário](#6-scripts-em-unity---nível-intermediário)
7. [Scripts em Unity - Nível Avançado](#7-scripts-em-unity---nível-avançado)
8. [Otimização e Boas Práticas](#8-otimização-e-boas-práticas)
9. [Projetos Práticos](#9-projetos-práticos)
10. [Referências e Recursos](#10-referências-e-recursos)

---

## 1. Introdução ao C# e Unity

### 1.1 O que é C#?
C# (pronuncia-se "C Sharp") é uma linguagem de programação moderna, orientada a objetos e type-safe desenvolvida pela Microsoft. É a linguagem principal utilizada no motor de jogos Unity.

**Características principais:**
- Fortemente tipada
- Orientada a objetos
- Gerenciamento automático de memória (Garbage Collection)
- Multi-paradigma
- Sintaxe limpa e expressiva

### 1.2 Por que C# no Unity?
- **Performance:** Excelente equilíbrio entre facilidade de uso e performance
- **Comunidade:** Grande comunidade e abundância de recursos
- **Integração:** Integração profunda com a engine Unity
- **Versatilidade:** Usado tanto para jogos mobile quanto para AAA

### 1.3 Configuração do Ambiente

#### Instalação do Unity Hub
1. Acesse unity.com/download
2. Baixe e instale o Unity Hub
3. Pelo Hub, instale a versão LTS (Long Term Support) mais recente
4. Durante a instalação, inclua:
   - Visual Studio Community (ou VS Code)
   - Suporte para Windows/Mac/Linux
   - Documentação

#### Configuração do Visual Studio
```xml
// No Visual Studio Installer, garanta que estes componentes estão instalados:
- Desenvolvimento para desktop com .NET
- Desenvolvimento de jogos com Unity
- Extensões do Unity
```

#### Primeiro Projeto Unity
1. Abra o Unity Hub
2. Clique em "New Project"
3. Selecione o template "2D Core" ou "3D Core"
4. Nomeie seu projeto
5. Clique em "Create"

---

## 2. Fundamentos da Linguagem C#

### 2.1 Sintaxe Básica

#### Estrutura de um Programa Console
```csharp
using System;  // Importando namespaces

namespace MeuPrimeiroPrograma
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
            
            // Aguarda input do usuário antes de fechar
            Console.ReadKey();
        }
    }
}
```

#### No Unity (Script básico)
```csharp
using UnityEngine;  // Namespace principal do Unity

public class MeuPrimeiroScript : MonoBehaviour  // MonoBehaviour é a classe base para scripts Unity
{
    // Start é chamado antes do primeiro frame
    void Start()
    {
        Debug.Log("Jogo iniciado!");
    }
    
    // Update é chamado uma vez por frame
    void Update()
    {
        Debug.Log("Frame atualizado");
    }
}
```

### 2.2 Variáveis e Tipos de Dados

#### Tipos Primitivos
```csharp
// Tipos numéricos
int vida = 100;                    // Inteiro (32 bits)
float velocidade = 5.5f;            // Ponto flutuante (32 bits) - note o 'f'
double precisao = 123.456789;       // Ponto flutuante (64 bits)
byte nivel = 255;                   // 0 a 255
short danoPequeno = 32000;          // Inteiro 16 bits
long distanciaGrande = 999999999L;  // Inteiro 64 bits

// Booleanos
bool estaVivo = true;
bool jogoPausado = false;

// Texto
string nomeJogador = "Herói";
char inicial = 'A';                  // Um único caractere

// No Unity (tipos especiais)
Vector3 posicao = new Vector3(1, 2, 3);
Quaternion rotacao = Quaternion.identity;
Color corVermelha = Color.red;
```

#### Declaração e Inicialização
```csharp
// Declaração
int pontuacao;

// Inicialização
pontuacao = 0;

// Declaração e inicialização juntos
int municao = 30;

// Múltiplas variáveis
int x = 10, y = 20, z = 30;

// Constantes (não podem ser alteradas)
const float GRAVIDADE = 9.8f;
const int VIDAS_MAXIMAS = 3;

// Inferência de tipo (var)
var nome = "João";      // string
var idade = 25;         // int
var altura = 1.75f;     // float
```

### 2.3 Operadores

#### Operadores Aritméticos
```csharp
int a = 10, b = 3;

int soma = a + b;           // 13
int subtracao = a - b;      // 7
int multiplicacao = a * b;  // 30
int divisao = a / b;        // 3 (divisão inteira)
int resto = a % b;          // 1 (módulo/resto)

float divisaoPrecisa = 10f / 3f;  // 3.333333

// Operadores de incremento/decremento
int contador = 0;
contador++;                  // pós-incremento: 1
++contador;                  // pré-incremento: 2
contador--;                  // 1
--contador;                  // 0
```

#### Operadores de Comparação
```csharp
int vida = 50;
int vidaMaxima = 100;

bool igual = (vida == vidaMaxima);           // false
bool diferente = (vida != vidaMaxima);       // true
bool maior = (vida > vidaMaxima);            // false
bool menor = (vida < vidaMaxima);            // true
bool maiorOuIgual = (vida >= vidaMaxima);    // false
bool menorOuIgual = (vida <= vidaMaxima);    // true
```

#### Operadores Lógicos
```csharp
bool temChave = true;
bool portaDestrancada = false;

// AND (E) - ambos precisam ser true
bool podeAbrir = temChave && portaDestrancada;  // false

// OR (OU) - pelo menos um precisa ser true
bool podeTentar = temChave || portaDestrancada;  // true

// NOT (NÃO) - inverte o valor
bool naoTemChave = !temChave;  // false

// Exemplo prático
int vida = 80;
bool temEscudo = true;
bool estaInvencivel = (vida > 0) || temEscudo;  // true
bool estaMorto = (vida <= 0) && !temEscudo;     // false
```

#### Operadores de Atribuição
```csharp
int valor = 10;

valor += 5;      // valor = valor + 5  (15)
valor -= 3;      // 12
valor *= 2;      // 24
valor /= 4;      // 6
valor %= 2;      // 0

// Operador ternário
int idade = 18;
string status = (idade >= 18) ? "Adulto" : "Menor";
```

### 2.4 Estruturas de Controle

#### If/Else
```csharp
int vida = 75;
int vidaMaxima = 100;

if (vida <= 0)
{
    Debug.Log("Jogador morreu!");
    GameOver();
}
else if (vida < 30)
{
    Debug.Log("Vida baixa!");
    TocarAlertaVidaBaixa();
}
else
{
    Debug.Log("Vida normal");
}

// If aninhado
if (temChave)
{
    if (portaTrancada)
    {
        Debug.Log("Abrindo porta com a chave");
    }
}
```

#### Switch
```csharp
string diaSemana = "segunda";

switch (diaSemana)
{
    case "segunda":
    case "terça":
    case "quarta":
    case "quinta":
    case "sexta":
        Debug.Log("Dia útil");
        break;
        
    case "sábado":
    case "domingo":
        Debug.Log("Fim de semana");
        break;
        
    default:
        Debug.Log("Dia inválido");
        break;
}

// Switch com pattern matching (C# 7+)
object obj = 42;
switch (obj)
{
    case int i when i > 0:
        Debug.Log($"Inteiro positivo: {i}");
        break;
    case string s:
        Debug.Log($"String: {s}");
        break;
    default:
        Debug.Log("Tipo desconhecido");
        break;
}
```

### 2.5 Loops (Estruturas de Repetição)

#### For Loop
```csharp
// Contagem crescente
for (int i = 0; i < 10; i++)
{
    Debug.Log($"Iteração: {i}");
}

// Contagem decrescente
for (int i = 10; i > 0; i--)
{
    Debug.Log($"Contagem regressiva: {i}");
}

// Loop com passo diferente
for (int i = 0; i <= 100; i += 10)
{
    Debug.Log($"Múltiplo de 10: {i}");
}

// Loop aninhado (tabuleiro)
for (int linha = 0; linha < 8; linha++)
{
    for (int coluna = 0; coluna < 8; coluna++)
    {
        Debug.Log($"Posição: [{linha},{coluna}]");
    }
}
```

#### While Loop
```csharp
int tentativas = 0;
int maxTentativas = 3;

while (tentativas < maxTentativas)
{
    Debug.Log($"Tentativa {tentativas + 1}");
    tentativas++;
    
    if (ConseguiuAbrirPorta())
    {
        Debug.Log("Porta aberta!");
        break;  // Sai do loop
    }
}

// While com condição complexa
bool jogoRodando = true;
while (jogoRodando)
{
    ProcessarInput();
    AtualizarJogo();
    Renderizar();
    
    if (JogadorSaiu())
    {
        jogoRodando = false;
    }
}
```

#### Do-While Loop
```csharp
// Executa pelo menos uma vez
int numero;
do
{
    numero = Random.Range(1, 11);  // 1 a 10
    Debug.Log($"Número sorteado: {numero}");
} while (numero != 7);

Debug.Log("Achou o 7!");
```

#### Foreach Loop
```csharp
string[] inimigos = { "Goblin", "Orc", "Dragão" };

foreach (string inimigo in inimigos)
{
    Debug.Log($"Encontrou: {inimigo}");
}

// Com coleções do Unity
List<GameObject> itensColetados = new List<GameObject>();
foreach (GameObject item in itensColetados)
{
    item.SetActive(false);
}
```

### 2.6 Arrays e Coleções

#### Arrays
```csharp
// Declaração e inicialização
int[] pontuacoes = new int[5];  // Array de 5 inteiros (todos 0)
pontuacoes[0] = 100;
pontuacoes[1] = 200;

// Inicialização direta
string[] inimigos = { "Goblin", "Orc", "Troll" };

// Array multidimensional
int[,] tabuleiro = new int[8, 8];
tabuleiro[0, 0] = 1;  // Primeira posição

// Array de arrays (jagged array)
int[][] matrizIrregular = new int[3][];
matrizIrregular[0] = new int[] { 1, 2 };
matrizIrregular[1] = new int[] { 3, 4, 5 };
```

#### Listas (List<T>)
```csharp
using System.Collections.Generic;

// Declaração
List<string> inventario = new List<string>();

// Adicionar itens
inventario.Add("Espada");
inventario.Add("Poção");
inventario.Add("Escudo");

// Inserir em posição específica
inventario.Insert(1, "Arco");  // [Espada, Arco, Poção, Escudo]

// Remover
inventario.Remove("Poção");
inventario.RemoveAt(0);  // Remove o primeiro

// Verificar existência
if (inventario.Contains("Espada"))
{
    Debug.Log("Tem espada!");
}

// Acessar por índice
string primeiroItem = inventario[0];

// Percorrer
foreach (string item in inventario)
{
    Debug.Log($"Item: {item}");
}

// Propriedades úteis
int quantidade = inventario.Count;
inventario.Sort();  // Ordenar
inventario.Clear(); // Limpar tudo
```

#### Dicionários (Dictionary<TKey, TValue>)
```csharp
using System.Collections.Generic;

// Criar dicionário
Dictionary<string, int> pontuacaoJogadores = new Dictionary<string, int>();

// Adicionar valores
pontuacaoJogadores.Add("João", 1000);
pontuacaoJogadores.Add("Maria", 1500);
pontuacaoJogadores["Pedro"] = 800;  // Também funciona

// Verificar existência
if (pontuacaoJogadores.ContainsKey("João"))
{
    int pontosJoao = pontuacaoJogadores["João"];
    Debug.Log($"Pontuação do João: {pontosJoao}");
}

// Tentar obter valor com segurança
if (pontuacaoJogadores.TryGetValue("Ana", out int pontosAna))
{
    Debug.Log($"Pontos da Ana: {pontosAna}");
}
else
{
    Debug.Log("Ana não encontrada");
}

// Iterar sobre o dicionário
foreach (KeyValuePair<string, int> par in pontuacaoJogadores)
{
    Debug.Log($"Jogador: {par.Key}, Pontos: {par.Value}");
}

// Remover
pontuacaoJogadores.Remove("Pedro");
```

### 2.7 Métodos (Funções)

#### Sintaxe Básica
```csharp
// Método simples sem retorno
void Saudacao()
{
    Debug.Log("Olá, mundo!");
}

// Método com parâmetros
void Saudar(string nome)
{
    Debug.Log($"Olá, {nome}!");
}

// Método com retorno
int Somar(int a, int b)
{
    int resultado = a + b;
    return resultado;
}

// Chamando métodos
Saudacao();
Saudar("João");
int soma = Somar(5, 3);  // soma = 8
```

#### Parâmetros Avançados
```csharp
// Parâmetros opcionais (devem vir por último)
void CriarJogador(string nome, int idade = 18, bool vip = false)
{
    Debug.Log($"Jogador: {nome}, Idade: {idade}, VIP: {vip}");
}

// Chamadas possíveis
CriarJogador("João");
CriarJogador("Maria", 25);
CriarJogador("Pedro", 30, true);

// Parâmetros nomeados
CriarJogador(nome: "Ana", vip: true, idade: 22);

// Parâmetros out (retornam múltiplos valores)
bool Dividir(int dividendo, int divisor, out int resultado, out int resto)
{
    if (divisor == 0)
    {
        resultado = 0;
        resto = 0;
        return false;
    }
    
    resultado = dividendo / divisor;
    resto = dividendo % divisor;
    return true;
}

// Usando out
if (Dividir(10, 3, out int res, out int rest))
{
    Debug.Log($"Resultado: {res}, Resto: {rest}");
}

// Parâmetros ref (passagem por referência)
void Dobrar(ref int numero)
{
    numero *= 2;
}

int valor = 5;
Dobrar(ref valor);  // valor agora é 10

// Parâmetros params (número variável de argumentos)
int SomarTodos(params int[] numeros)
{
    int soma = 0;
    foreach (int num in numeros)
    {
        soma += num;
    }
    return soma;
}

int total = SomarTodos(1, 2, 3, 4, 5);  // 15
```

#### Sobrecarga de Métodos
```csharp
public class Calculadora
{
    // Mesmo nome, parâmetros diferentes
    public int Somar(int a, int b)
    {
        return a + b;
    }
    
    public float Somar(float a, float b)
    {
        return a + b;
    }
    
    public int Somar(int a, int b, int c)
    {
        return a + b + c;
    }
    
    public string Somar(string a, string b)
    {
        return a + b;
    }
}
```

### 2.8 Tratamento de Exceções

```csharp
try
{
    // Código que pode gerar erro
    int divisor = 0;
    int resultado = 10 / divisor;
    
    // Acessar índice inexistente
    int[] numeros = new int[5];
    numeros[10] = 100;
}
catch (DivideByZeroException ex)
{
    Debug.LogError($"Erro de divisão por zero: {ex.Message}");
}
catch (IndexOutOfRangeException ex)
{
    Debug.LogError($"Índice fora do range: {ex.Message}");
}
catch (Exception ex)  // Captura qualquer exceção
{
    Debug.LogError($"Erro genérico: {ex.Message}");
}
finally
{
    // Sempre executa, mesmo com erro
    Debug.Log("Limpeza de recursos...");
}

// Lançando exceções
void SacarDinheiro(float valor)
{
    if (valor <= 0)
    {
        throw new ArgumentException("Valor deve ser positivo");
    }
    
    if (valor > saldo)
    {
        throw new InvalidOperationException("Saldo insuficiente");
    }
    
    saldo -= valor;
}
```

---

## 3. Programação Orientada a Objetos

### 3.1 Classes e Objetos

#### Definindo uma Classe
```csharp
using UnityEngine;

// Definição da classe
public class Jogador
{
    // Atributos (campos)
    public string nome;
    public int vida = 100;
    private float velocidade = 5f;
    protected bool estaVivo = true;
    
    // Construtor
    public Jogador()
    {
        nome = "Sem nome";
    }
    
    // Construtor com parâmetros
    public Jogador(string nomeJogador)
    {
        nome = nomeJogador;
    }
    
    // Métodos
    public void Andar()
    {
        Debug.Log($"{nome} está andando");
    }
    
    private void CalcularDano()
    {
        // Lógica interna
    }
    
    public int GetVida()
    {
        return vida;
    }
}

// Usando a classe
public class GameManager : MonoBehaviour
{
    void Start()
    {
        // Criando objetos (instâncias)
        Jogador jogador1 = new Jogador();
        jogador1.nome = "João";
        
        Jogador jogador2 = new Jogador("Maria");
        
        // Chamando métodos
        jogador1.Andar();
        
        Debug.Log($"Vida do jogador1: {jogador1.GetVida()}");
    }
}
```

### 3.2 Modificadores de Acesso

```csharp
public class ExemploModificadores
{
    // Acessível de qualquer lugar
    public string publico = "Todos veem";
    
    // Acessível apenas dentro desta classe
    private string privado = "Só eu vejo";
    
    // Acessível nesta classe e em classes filhas
    protected string protegido = "Eu e meus filhos";
    
    // Acessível dentro do mesmo assembly
    internal string interno = "Mesmo projeto";
    
    // Acessível em classes filhas de outros assemblies
    protected internal string protegidoInterno = "Filhos ou mesmo assembly";
    
    // Acessível apenas dentro do mesmo assembly
    private protected string privadoProtegido = "Mesmo assembly ou filho";
}
```

### 3.3 Propriedades (Getters e Setters)

```csharp
public class Personagem
{
    // Campo privado
    private int vida;
    private string nome;
    private float velocidade;
    
    // Propriedade completa
    public int Vida
    {
        get { return vida; }
        set 
        { 
            if (value >= 0)
                vida = value;
            else
                vida = 0;
        }
    }
    
    // Propriedade auto-implementada
    public string Nome { get; set; }
    
    // Propriedade com expressão body (C# 6+)
    public float Velocidade
    {
        get => velocidade;
        set => velocidade = value > 0 ? value : 0;
    }
    
    // Propriedade somente leitura
    public bool EstaVivo { get; private set; }
    
    // Propriedade calculada
    public int VidaEmPercentual 
    { 
        get { return (vida * 100) / vidaMaxima; }
    }
    
    private int vidaMaxima = 100;
    
    // Construtor
    public Personagem()
    {
        EstaVivo = true;
        vida = vidaMaxima;
    }
}

// Uso
Personagem heroi = new Personagem();
heroi.Nome = "Herói";
heroi.Vida = 75;
Debug.Log($"{heroi.Nome} tem {heroi.Vida} de vida");
```

### 3.4 Herança

```csharp
// Classe base
public class Inimigo
{
    public string nome;
    public int vida;
    protected float velocidade;
    
    public Inimigo()
    {
        nome = "Inimigo Genérico";
        vida = 100;
        velocidade = 5f;
    }
    
    public virtual void Atacar()
    {
        Debug.Log($"{nome} ataca genericamente");
    }
    
    public void ReceberDano(int dano)
    {
        vida -= dano;
        if (vida <= 0)
        {
            Morrer();
        }
    }
    
    protected virtual void Morrer()
    {
        Debug.Log($"{nome} morreu");
    }
}

// Classe derivada
public class Goblin : Inimigo
{
    public int furia;
    
    public Goblin()
    {
        nome = "Goblin";
        vida = 50;
        velocidade = 8f;
        furia = 0;
    }
    
    // Sobrescrevendo método virtual
    public override void Atacar()
    {
        Debug.Log($"{nome} ataca com adaga!");
        furia++;
    }
    
    // Usando base para chamar método da classe pai
    protected override void Morrer()
    {
        Debug.Log($"{nome} grita e morre!");
        base.Morrer();  // Chama o Morrer() da classe Inimigo
    }
}

// Classe derivada com construtor usando base
public class Orc : Inimigo
{
    public int forca;
    
    public Orc() : base()  // Chama construtor da classe base
    {
        nome = "Orc";
        vida = 150;
        forca = 10;
    }
    
    public Orc(int forcaExtra) : this()  // Chama construtor desta classe
    {
        forca += forcaExtra;
    }
    
    public override void Atacar()
    {
        Debug.Log($"{nome} ataca com machado causando {forca} de dano!");
    }
}
```

### 3.5 Polimorfismo

```csharp
public class GameController : MonoBehaviour
{
    void Start()
    {
        // Polimorfismo: tratar objetos diferentes de forma uniforme
        Inimigo[] inimigos = new Inimigo[]
        {
            new Goblin(),
            new Orc(),
            new Dragao()  // Outra classe derivada
        };
        
        // Cada inimigo ataca de sua própria maneira
        foreach (Inimigo inimigo in inimigos)
        {
            inimigo.Atacar();  // Comportamento polimórfico
        }
    }
}

// Exemplo com interfaces
public interface IDano
{
    void CausarDano(int quantidade);
    int CalcularDano();
}

public class Arma : IDano
{
    public int danoBase = 10;
    
    public void CausarDano(int quantidade)
    {
        Debug.Log($"Causando {quantidade} de dano");
    }
    
    public int CalcularDano()
    {
        return danoBase + Random.Range(0, 5);
    }
}

public class Magia : IDano
{
    public int poderMagico = 15;
    
    public void CausarDano(int quantidade)
    {
        Debug.Log($"Causando {quantidade} de dano mágico");
    }
    
    public int CalcularDano()
    {
        return poderMagico * 2;
    }
}
```

### 3.6 Abstração e Interfaces

#### Classes Abstratas
```csharp
// Classe abstrata - não pode ser instanciada diretamente
public abstract class ArmaBase
{
    public string nome;
    public int dano;
    
    // Método abstrato - deve ser implementado pelas classes filhas
    public abstract void Atacar();
    
    // Método concreto - pode ser usado por todas as classes filhas
    public void ExibirInfo()
    {
        Debug.Log($"Arma: {nome}, Dano: {dano}");
    }
}

public class Espada : ArmaBase
{
    public Espada()
    {
        nome = "Espada Longa";
        dano = 25;
    }
    
    public override void Atacar()
    {
        Debug.Log("Corte com espada!");
    }
}

public class Arco : ArmaBase
{
    public int flechas;
    
    public Arco()
    {
        nome = "Arco Élfico";
        dano = 15;
        flechas = 30;
    }
    
    public override void Atacar()
    {
        if (flechas > 0)
        {
            Debug.Log("Dispara flecha!");
            flechas--;
        }
        else
        {
            Debug.Log("Sem flechas!");
        }
    }
}
```

#### Interfaces
```csharp
// Interfaces definem contratos
public interface IDanoFisico
{
    int Forca { get; set; }
    void AtacarFisicamente();
}

public interface IDanoMagico
{
    int Mana { get; set; }
    void AtacarComMagia();
}

public interface IColetavel
{
    void Coletar(Jogador jogador);
    bool PodeColetar { get; }
}

// Classe pode implementar múltiplas interfaces
public class GuerreiroMago : MonoBehaviour, IDanoFisico, IDanoMagico
{
    public int Forca { get; set; }
    public int Mana { get; set; }
    
    void Start()
    {
        Forca = 10;
        Mana = 20;
    }
    
    public void AtacarFisicamente()
    {
        Debug.Log($"Ataque físico com força {Forca}");
    }
    
    public void AtacarComMagia()
    {
        if (Mana >= 5)
        {
            Debug.Log("Bola de fogo!");
            Mana -= 5;
        }
    }
}

// Implementação de IColetavel
public class Moeda : MonoBehaviour, IColetavel
{
    public int valor = 1;
    public bool PodeColetar => true;
    
    public void Coletar(Jogador jogador)
    {
        jogador.AdicionarMoedas(valor);
        Destroy(gameObject);
    }
}
```

### 3.7 Construtores e Destrutores

```csharp
public class Item
{
    public string nome;
    public int peso;
    public static int totalItens;  // Campo estático
    
    // Construtor estático (executado uma vez)
    static Item()
    {
        totalItens = 0;
        Debug.Log("Classe Item inicializada");
    }
    
    // Construtor padrão
    public Item()
    {
        nome = "Item Genérico";
        peso = 1;
        totalItens++;
    }
    
    // Construtor com parâmetros
    public Item(string nome, int peso)
    {
        this.nome = nome;
        this.peso = peso;
        totalItens++;
    }
    
    // Construtor de cópia
    public Item(Item outro)
    {
        nome = outro.nome;
        peso = outro.peso;
        totalItens++;
    }
    
    // Destrutor (finalizador)
    ~Item()
    {
        totalItens--;
        Debug.Log($"Item {nome} destruído");
    }
}

// Uso
Item espada = new Item("Espada", 5);
Item copia = new Item(espada);
```

---

## 4. Unity Essentials

### 4.1 Componentes e GameObjects

```csharp
using UnityEngine;

public class EntendendoGameObjects : MonoBehaviour
{
    void Start()
    {
        // Referenciando o próprio GameObject
        GameObject meuGameObject = this.gameObject;
        
        // Propriedades importantes
        string nome = meuGameObject.name;
        Transform transform = meuGameObject.transform;
        int layer = meuGameObject.layer;
        bool ativo = meuGameObject.activeInHierarchy;
        
        // Modificando propriedades
        meuGameObject.name = "NovoNome";
        meuGameObject.layer = 5;
        meuGameObject.SetActive(false);  // Desativa o objeto
        
        // Encontrando GameObjects
        GameObject inimigo = GameObject.Find("Inimigo");
        GameObject player = GameObject.FindWithTag("Player");
        GameObject[] todosInimigos = GameObject.FindGameObjectsWithTag("Enemy");
        
        // Criando GameObjects
        GameObject cubo = GameObject.CreatePrimitive(PrimitiveType.Cube);
        cubo.transform.position = new Vector3(0, 1, 0);
        
        // Adicionando componentes
        Rigidbody rb = cubo.AddComponent<Rigidbody>();
        BoxCollider collider = cubo.AddComponent<BoxCollider>();
        
        // Acessando componentes
        Transform t = GetComponent<Transform>();
        Rigidbody rigidbody = GetComponent<Rigidbody>();
        
        if (rigidbody != null)
        {
            rigidbody.mass = 2f;
        }
        
        // Múltiplos componentes do mesmo tipo
        BoxCollider[] colliders = GetComponents<BoxCollider>();
        
        // Componentes em filhos
        BoxCollider colliderFilho = GetComponentInChildren<BoxCollider>();
        BoxCollider[] collidersFilhos = GetComponentsInChildren<BoxCollider>();
        
        // Componentes em pais
        BoxCollider colliderPai = GetComponentInParent<BoxCollider>();
    }
}
```

### 4.2 Transformações

```csharp
using UnityEngine;

public class ManipulandoTransform : MonoBehaviour
{
    void Start()
    {
        // Posição
        Vector3 posicao = transform.position;  // Posição global
        Vector3 posicaoLocal = transform.localPosition;  // Posição relativa ao pai
        
        transform.position = new Vector3(10, 5, 0);
        transform.localPosition = Vector3.zero;
        
        // Rotação (em ângulos de Euler)
        Vector3 rotacao = transform.eulerAngles;
        transform.eulerAngles = new Vector3(0, 90, 0);
        
        // Rotação (Quaternion - mais complexo, mas melhor)
        Quaternion rot = transform.rotation;
        transform.rotation = Quaternion.identity;  // Sem rotação
        transform.rotation = Quaternion.Euler(0, 90, 0);
        
        // Escala
        Vector3 escala = transform.localScale;
        transform.localScale = new Vector3(2, 2, 2);
        
        // Movimentação
        transform.Translate(Vector3.forward * 5f);  // Move para frente
        transform.Translate(Vector3.right * 2f, Space.World);  // Move no espaço global
        
        // Rotação
        transform.Rotate(Vector3.up * 90f);  // Rotaciona 90 graus no eixo Y
        transform.Rotate(0, 90, 0, Space.World);  // Rotação global
        
        // Olhar para um ponto
        transform.LookAt(new Vector3(0, 0, 10));
        
        // Relações parentais
        Transform pai = transform.parent;
        transform.SetParent(null);  // Remove o pai
        transform.SetParent(GameObject.Find("Container").transform);
        
        // Iterando sobre filhos
        foreach (Transform filho in transform)
        {
            Debug.Log($"Filho: {filho.name}");
        }
        
        // Número de filhos
        int quantidadeFilhos = transform.childCount;
    }
    
    void Update()
    {
        // Movimento suave com Time.deltaTime
        float velocidade = 5f;
        transform.Translate(Vector3.forward * velocidade * Time.deltaTime);
        
        // Rotação suave
        float rotacaoVelocidade = 90f;
        transform.Rotate(Vector3.up * rotacaoVelocidade * Time.deltaTime);
        
        // Movimento em direção a um alvo
        Vector3 alvo = new Vector3(0, 0, 10);
        transform.position = Vector3.MoveTowards(transform.position, alvo, velocidade * Time.deltaTime);
        
        // Rotação suave para um alvo
        Quaternion rotacaoAlvo = Quaternion.Euler(0, 90, 0);
        transform.rotation = Quaternion.RotateTowards(transform.rotation, rotacaoAlvo, rotacaoVelocidade * Time.deltaTime);
        
        // Interpolação linear
        float t = Mathf.PingPong(Time.time, 1);
        transform.position = Vector3.Lerp(startPos, endPos, t);
    }
}
```

### 4.3 Input do Usuário

```csharp
using UnityEngine;

public class GerenciadorInput : MonoBehaviour
{
    void Update()
    {
        // Input de teclado
        if (Input.GetKey(KeyCode.W))  // Enquanto segura
        {
            transform.Translate(Vector3.forward * Time.deltaTime * 5f);
        }
        
        if (Input.GetKeyDown(KeyCode.Space))  // No frame em que pressiona
        {
            Pular();
        }
        
        if (Input.GetKeyUp(KeyCode.LeftShift))  // No frame em que solta
        {
            PararCorrer();
        }
        
        // Input de eixos (recomendado para movimento)
        float horizontal = Input.GetAxis("Horizontal");  // Valores suaves (-1 a 1)
        float vertical = Input.GetAxis("Vertical");
        
        Vector3 movimento = new Vector3(horizontal, 0, vertical);
        transform.Translate(movimento * Time.deltaTime * 5f);
        
        // Eixos com valores discretos (0, 0.5, 1)
        float rawHorizontal = Input.GetAxisRaw("Horizontal");
        
        // Input de mouse
        if (Input.GetMouseButtonDown(0))  // Botão esquerdo
        {
