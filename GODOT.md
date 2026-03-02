
# Manual Completo de GDScript (Godot 4.x)

## Sumário

1.  [Introdução ao GDScript](#1-introdução-ao-gdscript)
    - [O que é GDScript?](#o-que-é-gdscript)
    - [GDScript 2.0](#gdscript-20)
    - [Sua Primeira Script](#sua-primeira-script)
2.  [Conceitos Fundamentais do Godot](#2-conceitos-fundamentais-do-godot)
    - [Nós e Cenas](#nós-e-cenas)
    - [Árvore de Cena](#árvore-de-cena)
    - [Sinais](#sinais)
3.  [Sintaxe Básica](#3-sintaxe-básica)
    - [Estrutura de um Arquivo .gd](#estrutura-de-um-arquivo-gd)
    - [Comentários](#comentários)
    - [Identificadores e Palavras-chave](#identificadores-e-palavras-chave)
4.  [Variáveis, Constantes e Enums](#4-variáveis-constantes-e-enums)
    - [Declarando Variáveis (`var`)](#declarando-variáveis-var)
    - [Constantes (`const`)](#constantes-const)
    - [Enumerações (`enum`)](#enumerações-enum)
    - [Anotações (`@export`, `@onready`)](#anotações-export-onready)
5.  [Tipos de Dados](#5-tipos-de-dados)
    - [Tipos Básicos](#tipos-básicos)
    - [Tipos Vetoriais](#tipos-vetoriais)
    - [Tipos Especiais](#tipos-especiais)
    - [Tipagem Estática (Type Hints)](#tipagem-estática-type-hints)
6.  [Operadores](#6-operadores)
7.  [Estruturas de Controle de Fluxo](#7-estruturas-de-controle-de-fluxo)
    - [Condicionais (`if`, `elif`, `else`)](#condicionais-if-elif-else)
    - [`match` (Switch)](#match-switch)
    - [Loops (`for`, `while`)](#loops-for-while)
    - [Controle de Loop (`break`, `continue`)](#controle-de-loop-break-continue)
8.  [Funções](#8-funções)
    - [Declarando Funções (`func`)](#declarando-funções-func)
    - [Retornando Valores (`return`)](#retornando-valores-return)
    - [Funções Virtuais Especiais](#funções-virtuais-especiais)
    - [Parâmetros Padrão](#parâmetros-padrão)
9.  [Estruturas de Dados: Arrays e Dicionários](#9-estruturas-de-dados-arrays-e-dicionários)
    - [Arrays](#arrays)
    - [Dicionários](#dicionários)
10. [Programação Orientada a Objetos (POO)](#10-programação-orientada-a-objetos-poo)
    - [Cada Arquivo é uma Classe](#cada-arquivo-é-uma-classe)
    - [Herança (`extends`)](#herança-extends)
    - [Registrando uma Classe Global (`class_name`)](#registrando-uma-classe-global-class_name)
    - [Classes Internas](#classes-internas)
    - [Polimorfismo e `super`](#polimorfismo-e-super)
11. [Sinais (Signal)](#11-sinais-signal)
    - [Declarando e Emitindo Sinais](#declarando-e-emitindo-sinais)
    - [Conectando Sinais](#conectando-sinais)
    - [Aguardando Sinais com `await`](#aguardando-sinais-com-await)
12. [Tópicos Avançados](#12-tópicos-avançados)
    - [Tratamento de Erros com `assert`](#tratamento-de-erros-com-assert)
    - [Pré-carregamento vs Carregamento Dinâmico (`preload` vs `load`)](#pré-carregamento-vs-carregamento-dinâmico-preload-vs-load)
    - [Autoloads (Singletons)](#autoloads-singletons)

---

## 1. Introdução ao GDScript

### O que é GDScript?
GDScript é a linguagem de script principal do Godot Engine. É uma linguagem de alto nível, orientada a objetos, de tipagem gradual e imperativa, com uma sintaxe indentada (similar ao Python). Seu principal objetivo é ser perfeitamente integrada ao Godot, oferecendo uma experiência otimizada e fluida para a criação de conteúdo e lógica de jogos .

Apesar da semelhança superficial com Python, o GDScript é completamente independente e foi projetado especificamente para trabalhar com as APIs e os conceitos do Godot, como nós, cenas e sinais .

### GDScript 2.0
Com o lançamento do Godot 4, o GDScript foi atualizado para a versão 2.0. Esta nova versão trouxe melhorias significativas, incluindo:
- **Sintaxe aprimorada:** Nova palavra-chave `super` para chamar métodos da classe pai, substituindo o antigo `.` (ponto) .
- **Gerenciamento de corrotinas:** Substituição do `yield` pelo mais intuitivo `await` para aguardar sinais e corrotinas .
- **Tipagem mais robusta:** Melhor suporte e inferência de tipos.
- **Novas anotações:** Uso de `@` para anotações como `@export` e `@onready`, tornando o código mais claro .

### Sua Primeira Script
Para criar um script no Godot, selecione um nó na sua cena, clique no botão "Anexar Script" (ícone de documento com um '+' ) no topo da cena ou no painel do nó. O Godot gera uma estrutura básica que você pode modificar. Um simples "Hello, World!" em GDScript seria assim:

```gdscript
extends Node2D  # Supondo que o script esteja anexado a um Node2D

func _ready():
    # A função _ready é chamada quando o nó entra na árvore de cena pela primeira vez.
    print("Olá, Mundo do Godot!")
```

Ao rodar a cena, a mensagem será impressa no console do editor (painel inferior) .

## 2. Conceitos Fundamentais do Godot

Antes de mergulhar fundo na linguagem, é crucial entender os conceitos com os quais ela interage diretamente.

### Nós e Cenas
- **Nó (Node):** É o bloco de construção mais básico do Godot. Representa um elemento do jogo, como um sprite, uma câmera, um som, um personagem, etc. Cada nó tem uma função específica e pode ter outros nós como filhos .
- **Cena (Scene):** É um conjunto organizado de nós em uma árvore. Uma cena pode ser um personagem, um nível, uma interface de usuário (UI), um item de inventário... qualquer coisa. As cenas são aninháveis, ou seja, você pode instanciar uma cena dentro de outra .

### Árvore de Cena
Quando seu jogo está rodando, todas as cenas (e seus nós) se combinam em uma única estrutura chamada **Árvore de Cena (Scene Tree)**. A Árvore de Cena gerencia o fluxo do jogo, propagando eventos, atualizando a lógica (`_process`) e a física (`_physics_process`) para todos os nós .

### Sinais
Sinais são o sistema de mensagens do Godot, uma implementação do padrão **Observer**. Um nó emite um sinal quando algo acontece (ex: um botão é pressionado, dois corpos colidem, um timer acaba). Outros nós podem se "conectar" a esse sinal para serem notificados e executar uma função em resposta. Isso permite um acoplamento fraco entre os nós, tornando o código mais modular e organizado .

## 3. Sintaxe Básica

### Estrutura de um Arquivo .gd
```gdscript
# (Opcional) Define um nome global para esta classe/script
class_name Jogador

# (Obrigatório) Define de qual nó ou classe este script herda
extends CharacterBody2D

# (Opcional) Ícone personalizado para o script no editor
@icon("res://icone_jogador.svg")

# 1. Sinais
signal morreu

# 2. Variáveis de membro (e constantes/enums)
var velocidade = 300
@export var pulo_forca = 600
const GRAVIDADE = 980

# 3. Funções (incluindo as virtuais)
func _ready():
    add_to_group("jogadores")

func _physics_process(delta):
    movimento(delta)

# 4. Funções internas (lógica do jogo)
func movimento(delta):
    # ... lógica de movimento ...
    pass
```

### Comentários
- `#` para comentários de linha única.
- Não há suporte nativo para comentários de múltiplas linhas (use vários `#`).

```gdscript
# Isto é um comentário
var vida = 100  # Isto também é um comentário
```

### Identificadores e Palavras-chave
- **Identificadores:** Nomes de variáveis, funções, etc. Devem começar com uma letra ou `_` e podem conter letras, números e `_`. São **case-sensitive** (`velocidade` é diferente de `Velocidade`) .
- **Palavras-chave:** São reservadas pela linguagem e não podem ser usadas como identificadores. Exemplos: `if`, `for`, `while`, `func`, `var`, `const`, `extends`, `class_name`, `return`, `break`, `continue`, `pass`, `true`, `false`, `self`, `super`, `await`, etc .

## 4. Variáveis, Constantes e Enums

### Declarando Variáveis (`var`)
```gdscript
var nome = "Herói"                 # Tipo inferido como String
var idade = 30                      # Tipo inferido como int
var altura = 1.75                   # Tipo inferido como float
var itens = []                       # Array vazio
var dados = {}                       # Dicionário vazio
var ponto = Vector2.ZERO             # Usando uma constante
var vida_maxima: int = 100           # Com dica de tipo explícita (type hint)
var posicao_inicial := Vector2(100, 50) # Operador ':=' para inferir o tipo pelo valor
```

### Constantes (`const`)
O valor de uma constante **não pode** ser alterado após ser definido.
```gdscript
const VELOCIDADE_MAXIMA = 600
const NOME_JOGO = "Minha Aventura"
const GRAVIDADE = 9.8
```

### Enumerações (`enum`)
Criam um conjunto nomeado de inteiros, útil para tornar o código mais legível .
```gdscript
enum Estado {PARADO, ANDANDO, CORRENDO, PULANDO}
enum Direcao {ESQUERDA = -1, DIREITA = 1} # Você pode atribuir valores personalizados

var estado_atual = Estado.PARADO
```

### Anotações (`@export`, `@onready`)
As anotações (que começam com `@`) são uma forma moderna de adicionar metadados às variáveis, substituindo as antigas palavras-chave como `export` e `onready` .

- **`@export`**: Torna a variável visível e editável no painel Inspector do editor. Permite ajustar valores sem reabrir o script.
    ```gdscript
    @export var velocidade = 300
    @export var textura_sprite: Texture2D
    @export_range(0, 100, 1) var vida = 100 # Exporta como um slider
    ```
- **`@onready`**: Adia a inicialização da variável até que o nó e seus filhos estejam prontos na árvore de cena. Essencial para acessar outros nós com `$`.
    ```gdscript
    @onready var sprite = $Sprite2D          # Encontra o nó filho "Sprite2D"
    @onready var arma = $"/root/NodePath/Arma" # Caminho absoluto
    @onready var label = %LabelNome           # Usando o operador % para nós únicos
    ```
    O operador `%` (e o `$` para caminhos) é um açúcar sintático para `get_node()` .

## 5. Tipos de Dados

Os tipos básicos em GDScript são passados por valor (cópias são criadas), exceto `Array` e `Dictionary`, que são passados por referência .

### Tipos Básicos
- **`null`**: Representa a ausência de dados .
- **`bool`**: Valores `true` ou `false` .
- **`int`**: Números inteiros (ex: `-10`, `0`, `42`). Armazenado como 64 bits .
- **`float`**: Números de ponto flutuante (ex: `3.14`, `-0.5`, `2.0`). Armazenado como 64 bits .
- **`String`**: Sequência de caracteres Unicode. Pode ser definida com `"aspas duplas"` ou `'aspas simples'`. Suporta strings multilinha com `"""`  e strings "cruas" (raw strings) com `r"..."` que ignoram sequências de escape .

### Tipos Vetoriais
- **`Vector2`**, **`Vector3`**: Representam coordenadas 2D (x, y) e 3D (x, y, z). Usados extensivamente para posição, movimento, etc .
    ```gdscript
    var pos = Vector2(100, 200)
    pos.x += 10
    var alvo = Vector3(1, 0, 0)
    ```

### Tipos Especiais
- **`NodePath`**: Caminho para um nó na árvore de cena. Pode ser escrito como `"caminho/para/no"` ou com o literal `@"caminho/para/no"` .
- **`RID`**: (Resource ID) Identificador único para recursos do servidor (pouco usado no dia a dia).
- **`Signal`**: Tipo para uma referência a um sinal.

### Tipagem Estática (Type Hints)
GDScript é dinamicamente tipado por padrão, mas suporta tipagem estática opcional (tipagem gradual), o que melhora a performance, a autocompleção e previne erros .
```gdscript
var velocidade_atual: float = 0.0
var nome_jogador: String
var lista_de_inimigos: Array[Node] = [] # Array tipado (Godot 4)
var dados_do_jogo: Dictionary

func processar_dano(quantidade: int, causador: Node) -> bool:
    vida -= quantidade
    print("Dano recebido de ", causador.name)
    return vida <= 0
```

## 6. Operadores

O GDScript suporta uma variedade de operadores, com precedência similar ao C++ .

| Categoria | Operadores | Notas |
| :--- | :--- | :--- |
| **Atribuição** | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, etc. | Menor precedência . |
| **Aritméticos** | `+`, `-`, `*`, `/`, `%` (resto), `**` (potência) | Divisão de `int` resulta em `int` truncado (ex: `5/2 = 2`). Use `float` para decimais . `%` não funciona com `float` (use `fmod()`) . |
| **Comparação** | `==`, `!=`, `<`, `>`, `<=`, `>=` | . |
| **Lógicos** | `and` (`&&`), `or` (`||`), `not` (`!`) | As versões com palavras-chave são mais idiomáticas . |
| **Bitwise** | `&`, `\|`, `^`, `~`, `<<`, `>>` | Operações a nível de bit . |
| **Outros** | `is` (teste de tipo), `as` (conversão de tipo), `in` (pertinência), `$` (`get_node`), `%` (`get_node` para único), `&` (StringName) | . |

**Nota sobre `in`**: Verifica se um valor existe em uma string, array, dicionário (chave) ou range .
```gdscript
if "chave" in um_dicionario:
    print("Chave encontrada!")

if 3 in [1, 2, 3, 4]:
    print("3 está no array")
```

## 7. Estruturas de Controle de Fluxo

### Condicionais (`if`, `elif`, `else`)
A indentação define os blocos de código .
```gdscript
var pontuacao = 75

if pontuacao >= 90:
    print("Nota A")
elif pontuacao >= 80:
    print("Nota B")
elif pontuacao >= 70:
    print("Nota C")
else:
    print("Nota F")
```

### `match` (Switch)
Similar ao `switch` de outras linguagens, mas mais poderoso, permitindo padrões complexos .
```gdscript
var tipo_inimigo = "goblin"

match tipo_inimigo:
    "goblin":
        print("Inimigo fraco, mas rápido.")
    "troll":
        print("Inimigo forte, mas lento.")
    "chefe":
        print("Chefe do nível!")
    _:  # O padrão (underscore) equivale ao 'default'
        print("Tipo de inimigo desconhecido.")
```

### Loops (`for`, `while`)
```gdscript
# for loop com range (0 a 4)
for i in range(5):
    print(i)

# for loop iterando sobre um array
var frutas = ["maca", "banana", "uva"]
for fruta in frutas:
    print("Eu gosto de ", fruta)

# while loop
var contador = 3
while contador > 0:
    print("Contagem regressiva: ", contador)
    contador -= 1
```

### Controle de Loop (`break`, `continue`)
- **`break`**: Sai imediatamente do loop .
- **`continue`**: Pula para a próxima iteração do loop .

## 8. Funções

### Declarando Funções (`func`)
```gdscript
func saudacao(nome: String) -> void:
    print("Olá, ", nome, "!")

func somar(a: int, b: int) -> int:
    return a + b

func calcular_media(valores: Array[float]) -> float:
    var soma = 0.0
    for valor in valores:
        soma += valor
    return soma / valores.size()
```

### Retornando Valores (`return`)
Usado para sair de uma função e, opcionalmente, retornar um valor. Funções sem `return` explícito retornam `null`.

### Funções Virtuais Especiais
São funções chamadas automaticamente pelo motor .
- **`_init()`**: Chamado quando o objeto é criado na memória.
- **`_ready()`**: Chamado quando o nó entra na árvore de cena pela primeira vez (e todos os seus filhos estão prontos) .
- **`_process(delta)`**: Chamado a cada quadro (frame). O parâmetro `delta` é o tempo em segundos desde o último quadro .
- **`_physics_process(delta)`**: Chamado a cada passo de física (por padrão, 60 vezes por segundo). Use para movimento e lógica que interage com a física .
- **`_input(event)`**: Chamado para cada evento de entrada (teclado, mouse, tela).

### Parâmetros Padrão
Você pode definir valores padrão para parâmetros, tornando-os opcionais na chamada .
```gdscript
func criar_projetil(tipo = "padrao", velocidade = 400, dano = 10):
    print("Criando ", tipo, " com velocidade ", velocidade)

# Chamadas possíveis:
criar_projetil()
criar_projetil("laser", 1000)
criar_projetil("missil", 500, 50)
```

## 9. Estruturas de Dados: Arrays e Dicionários

### Arrays
São listas ordenadas de valores, que podem ser de qualquer tipo (inclusive outros arrays) .
```gdscript
var inventario = ["espada", "pocao", "escudo"]
print(inventario[0])  # Acessa "espada"

inventario.append("chave")        # Adiciona ao final
inventario.remove_at(1)            # Remove o elemento no índice 1 ("pocao")
var primeiro_item = inventario.pop_front() # Remove e retorna o primeiro item

# Arrays tipados (Godot 4)
var numeros: Array[int] = [10, 20, 30]
numeros.append(40) # OK
# numeros.append("texto") # Erro!
```
Os arrays em GDScript são **passados por referência**. Se você atribuir um array a outra variável, ambas apontarão para o mesmo dado na memória .

### Dicionários
Armazenam pares de chave-valor. As chaves podem ser de qualquer tipo, mas geralmente são `String` ou `int` .
```gdscript
var jogador = {
    "nome": "Ari",
    "vida": 100,
    "posicao": Vector2(0, 0),
    "inventario": ["pocao"]
}

print(jogador["nome"])   # Acessa "Ari"
jogador["vida"] = 90      # Modifica o valor
jogador["ouro"] = 50      # Adiciona um novo par chave-valor

# Sintaxe alternativa (quando a chave é um identificador válido)
var outro_dict = {nome = "Bia", nivel = 5}
print(outro_dict.nome) # Acessa "Bia" usando ponto (açúcar sintático)
```
Assim como os arrays, dicionários também são **passados por referência** .

## 10. Programação Orientada a Objetos (POO)

### Cada Arquivo é uma Classe
No Godot, cada arquivo de script (`.gd`) define uma nova classe. O nome do arquivo (sem a extensão) é o nome da classe, a menos que você use `class_name` .

### Herança (`extends`)
Todo script que é anexado a um nó deve herdar de uma classe de nó (ex: `extends Node2D`, `extends Sprite`). Scripts "solteiros" podem herdar de `Resource` ou `Object` .
```gdscript
# Script: Inimigo.gd
extends CharacterBody2D

var vida = 100
```

```gdscript
# Script: Goblin.gd (herda de Inimigo.gd)
extends "res://Inimigo.gd" # Ou usa class_name se registrado

func _ready():
    vida = 50 # Sobrescreve o valor herdado
```

### Registrando uma Classe Global (`class_name`)
A palavra-chave `class_name` (seguida pelo nome da classe) torna seu script acessível globalmente, como um tipo nativo do Godot. Você pode usá-la para criar novas instâncias ou como dica de tipo em qualquer outro script .
```gdscript
# Arquivo: "Personagem.gd"
class_name Personagem
extends Node

var nome: String
```

```gdscript
# Outro script qualquer
var meu_personagem: Personagem = Personagem.new()
meu_personagem.nome = "Arthur"
```

### Classes Internas
Você pode definir classes dentro de outras classes/scripts usando a palavra-chave `class`. Elas só são acessíveis dentro do escopo do script pai .
```gdscript
class DadoInterno:
    var valor = 0
    
    func mostrar():
        print("Valor interno: ", valor)

# Usando a classe interna
func _ready():
    var meu_dado = DadoInterno.new()
    meu_dado.mostrar()
```

### Polimorfismo e `super`
Quando você sobrescreve um método da classe pai, pode chamar a implementação original usando `super` .
```gdscript
# Classe Pai: Animal.gd
func fazer_som():
    print("*som genérico*")
```

```gdscript
# Classe Filho: Cachorro.gd
extends "res://Animal.gd"

func fazer_som():
    super() # Chama Animal.fazer_som() primeiro
    print("Au Au!")
```

## 11. Sinais (Signal)

### Declarando e Emitindo Sinais
```gdscript
# Dentro do script do seu nó
signal morreu                        # Sinal simples
signal vida_alterada(valor_anterior, valor_novo) # Sinal com parâmetros
signal item_pegou(nome_item: String) # Com dica de tipo (Godot 4)

var vida = 100

func tomar_dano(quantidade):
    var vida_antiga = vida
    vida -= quantidade
    vida_alterada.emit(vida_antiga, vida) # Emite o sinal com os valores
    if vida <= 0:
        morreu.emit() # Emite o sinal
```

### Conectando Sinais
**Via Editor:** Selecione o nó que emite o sinal, vá até a aba "Node" ao lado do Inspector, encontre o sinal (ex: `morreu`), clique duas vezes e escolha o nó que vai receber e o método que será chamado.

**Via Código:** Use o método `connect` do sinal.
```gdscript
# No script do nó que quer receber o sinal
func _ready():
    # Conecta o sinal 'morreu' do inimigo à função '_on_inimigo_morreu' deste nó
    $Inimigo.morreu.connect(_on_inimigo_morreu)

func _on_inimigo_morreu():
    print("O inimigo morreu! Eu recebi o sinal.")
    queue_free() # Por exemplo, remove o inimigo
```

### Aguardando Sinais com `await`
A palavra-chave `await` pode ser usada para esperar um sinal ser emitido, pausando a execução da função sem travar o jogo .
```gdscript
func iniciar_temporizador():
    print("Temporizador iniciado")
    await $Timer.timeout  # Espera o sinal 'timeout' do Timer
    print("Tempo esgotado!")
```

## 12. Tópicos Avançados

### Tratamento de Erros com `assert`
`assert(condicao, mensagem)` verifica se uma condição é verdadeira. Se for falsa, o jogo para em modo de debug e exibe a mensagem de erro. É útil para encontrar bugs durante o desenvolvimento .
```gdscript
func dividir(a, b):
    assert(b != 0, "Divisor não pode ser zero!")
    return a / b
```

### Pré-carregamento vs Carregamento Dinâmico (`preload` vs `load`)
- **`preload`**: Carrega um recurso ou cena **em tempo de compilação**. É mais rápido e gera erros imediatamente se o recurso não for encontrado. Use para recursos que são essenciais e sempre necessários .
    ```gdscript
    const CENA_INIMIGO = preload("res://inimigo.tscn")
    ```
- **`load`**: Carrega um recurso **em tempo de execução**. Útil para carregar recursos opcionais ou níveis dinamicamente. É mais lento, mas oferece flexibilidade.
    ```gdscript
    var cena_fase = load("res://fases/fase_" + str(numero_fase) + ".tscn")
    ```

### Autoloads (Singletons)
O Autoload é um recurso do Godot que permite carregar uma cena ou script automaticamente quando o jogo inicia, tornando-o globalmente acessível (como um Singleton). É ótimo para gerenciadores de jogo, dados globais (como pontuação ou vida entre fases) ou áudio.

Para configurar, vá em Projeto > Configurações do Projeto > Guia AutoLoad, escolha o arquivo da cena/script e dê um nome (ex: "GameManager"). A partir de qualquer script, você pode acessá-lo diretamente: `GameManager.pontuacao = 100`.
