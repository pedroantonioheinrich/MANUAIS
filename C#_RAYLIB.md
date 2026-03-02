## 📖 Índice

1.  [Introdução](#introdução)
2.  [Instalação e Configuração](#instalação-e-configuração)
    - [Pré-requisitos](#pré-requisitos)
    - [Criando um Projeto e Instalando o Pacote NuGet](#criando-um-projeto-e-instalando-o-pacote-nuget)
    - [Estrutura do Projeto](#estrutura-do-projeto)
3.  [Seu Primeiro Programa: "Hello, World!"](#seu-primeiro-programa-hello-world)
    - [Explicação do Código](#explicação-do-código)
4.  [Módulos Principais da Raylib](#módulos-principais-da-raylib)
    - [Core (Inicialização e Gerenciamento de Janela)](#core-inicialização-e-gerenciamento-de-janela)
    - [Gráficos (2D e 3D)](#gráficos-2d-e-3d)
    - [Entrada (Teclado, Mouse, Gamepad)](#entrada-teclado-mouse-gamepad)
    - [Áudio](#áudio)
    - [Texto](#texto)
5.  [Particularidades do Raylib-cs em C#](#particularidades-do-raylib-cs-em-c)
6.  [Exemplos Práticos](#exemplos-práticos)
    - [Desenhando e Movimentando uma Forma](#desenhando-e-movimentando-uma-forma)
    - [Carregando e Desenhando uma Textura (Imagem)](#carregando-e-desenhando-uma-textura-imagem)
    - [Tocando um Som Simples](#tocando-um-som-simples)
7.  [Próximos Passos e Recursos](#próximos-passos-e-recursos)

---

## 1. Introdução

**Raylib** é uma biblioteca altamente popular para programação de jogos e multimídia, conhecida por sua simplicidade e design focado em aprendizado. Escrita em C, ela oferece uma vasta gama de funcionalidades sem a complexidade de motores de jogos gigantescos .

**Raylib-cs** é o projeto que traz essa simplicidade para o ecossistema C#. São *bindings* (ligações) que permitem que você chame as funções do Raylib diretamente do seu código C# de forma natural e eficiente, como se fosse uma biblioteca nativa da linguagem .

Com Raylib-cs, você pode criar desde pequenos protótipos e jogos 2D/3D até ferramentas interativas e visuais, tudo com uma curva de aprendizado suave e performance respeitável.

## 2. Instalação e Configuração

A maneira mais simples e recomendada de começar é através do gerenciador de pacotes NuGet.

### Pré-requisitos

- **.NET SDK:** Versão 5.0, 6.0 ou superior instalada no seu sistema . Você pode verificar a instalação com o comando `dotnet --version` no seu terminal.
- **Um Editor de Código/IDE:** Visual Studio 2022, Visual Studio Code (com a extensão C#), JetBrains Rider, ou qualquer outro de sua preferência.

### Criando um Projeto e Instalando o Pacote NuGet

Abra seu terminal e siga os passos abaixo:

1.  **Crie uma nova pasta para o seu projeto e entre nela:**
    ```bash
    mkdir MeuPrimeiroJogo
    cd MeuPrimeiroJogo
    ```

2.  **Crie um novo projeto console:**
    ```bash
    dotnet new console
    ```

3.  **Adicione o pacote Raylib-cs ao projeto:**
    ```bash
    dotnet add package Raylib-cs
    ```
    Este comando irá baixar e referenciar a versão mais recente e estável do Raylib-cs. Para versões específicas, você pode consultar a [página do pacote no NuGet](https://www.nuget.org/packages/Raylib-cs/) .

4.  **Pronto!** A estrutura básica do seu projeto está criada e a biblioteca instalada. As bibliotecas nativas (`raylib.dll` no Windows, `libraylib.so` no Linux, etc.) são automaticamente gerenciadas pelo pacote NuGet .

### Estrutura do Projeto

Após a instalação, a estrutura da sua pasta `MeuPrimeiroJogo` será semelhante a esta:

```
MeuPrimeiroJogo/
├── obj/                 # Arquivos temporários de compilação
├── bin/                 # Executáveis e bibliotecas compiladas
├── MeuPrimeiroJogo.csproj # Arquivo de projeto do .NET
└── Program.cs           # Seu arquivo de código principal (ponto de entrada)
```

## 3. Seu Primeiro Programa: "Hello, World!"

Vamos abrir o arquivo `Program.cs` e substituir seu conteúdo pelo código abaixo. Este é o exemplo clássico que abre uma janela e desenha um texto .

```csharp
using Raylib_cs;

namespace MeuPrimeiroJogo;

class Program
{
    public static void Main()
    {
        // 1. Inicializa a janela com largura 800, altura 480 e um título
        Raylib.InitWindow(800, 480, "Meu Primeiro Jogo com Raylib-cs");

        // 2. Game Loop (roda até que a janela seja fechada)
        while (!Raylib.WindowShouldClose())
        {
            // 3. Começa a desenhar o próximo frame
            Raylib.BeginDrawing();

            // 4. Limpa o fundo da tela com uma cor
            Raylib.ClearBackground(Color.White);

            // 5. Desenha um texto na posição (12, 12) com tamanho 20 e cor preta
            Raylib.DrawText("Hello, mundo Raylib-cs!", 12, 12, 20, Color.Black);

            // 6. Finaliza o desenho do frame
            Raylib.EndDrawing();
        }

        // 7. Fecha a janela e libera recursos
        Raylib.CloseWindow();
    }
}
```

Para executar, volte ao terminal e digite:

```bash
dotnet run
```

Uma janela com o fundo branco e a mensagem "Hello, mundo Raylib-cs!" deve aparecer.

### Explicação do Código

- `using Raylib_cs;`: Importa o namespace com todas as funcionalidades da Raylib .
- `Raylib.InitWindow()`: Cria a janela da aplicação. Deve ser chamada uma única vez, no início .
- `while (!Raylib.WindowShouldClose())`: Este é o **game loop** principal. Ele se repete a cada frame até que o usuário tente fechar a janela. É aqui que toda a lógica do jogo e desenho acontece .
- `Raylib.BeginDrawing()` / `Raylib.EndDrawing()`: Delimitam o bloco de desenho de um frame. Tudo o que for desenhado entre essas duas chamadas será renderizado na tela quando `EndDrawing` for chamado .
- `Raylib.ClearBackground()`: Limpa o frame atual com uma cor sólida, apagando o desenho do frame anterior .
- `Raylib.DrawText()`: Desenha um texto na tela. Você precisa especificar o texto, a posição (X, Y), o tamanho da fonte e a cor .
- `Raylib.CloseWindow()`: Fecha a janela e libera os recursos alocados pela Raylib. Deve ser chamada no final do programa .

## 4. Módulos Principais da Raylib

A Raylib é organizada em módulos. Abaixo, alguns dos mais importantes no contexto do Raylib-cs:

### Core (Inicialização e Gerenciamento de Janela)

Este módulo contém as funções essenciais para configuração e controle da aplicação. Já vimos algumas:
- `InitWindow()`, `CloseWindow()`, `WindowShouldClose()`.
- `SetTargetFPS(int fps)`: Define a taxa de quadros por segundo desejada.
- `GetFrameTime()`: Retorna o tempo decorrido desde o último frame (em segundos). Essencial para movimentação independente de FPS.
- `GetScreenWidth()`, `GetScreenHeight()`: Obtém as dimensões da janela.

### Gráficos (2D e 3D)

A Raylib tem um conjunto muito rico de funções de desenho.

**2D:**
- `DrawPixel()`, `DrawLine()`, `DrawCircle()`, `DrawRectangle()`, `DrawTriangle()`.
- `DrawTexture()`, `DrawTexturePro()`: Para desenhar imagens (texturas) com suporte a rotação, escala e recorte.
- Cores são representadas pela struct `Color`. Existem várias pré-definidas: `Color.Red`, `Color.Blue`, `Color.Green`, `Color.White`, `Color.Black`, etc. .

**3D:**
- `DrawCube()`, `DrawSphere()`, `DrawModel()`.
- Funções para câmera (`Camera3D`), luzes e shaders.

### Entrada (Teclado, Mouse, Gamepad)

Facilita a captura de input do usuário.

- `IsKeyPressed(KeyboardKey.KEY_SPACE)`: Retorna `true` apenas no frame em que a tecla foi pressionada.
- `IsKeyDown(KeyboardKey.KEY_RIGHT)`: Retorna `true` enquanto a tecla estiver sendo segurada .
- `GetMousePosition()`: Retorna as coordenadas atuais do mouse (`Vector2`).
- `IsMouseButtonPressed(MouseButton.MOUSE_BUTTON_LEFT)`.
- `GetGamepadAxisMovement()`, `IsGamepadButtonPressed()`.

### Áudio

Para carregar e reproduzir sons e músicas.

- `InitAudioDevice()`: Inicializa o subsistema de áudio.
- `LoadSound("caminho/para/som.wav")`: Carrega um arquivo de som.
- `PlaySound(Sound som)`: Toca o som carregado.
- `LoadMusicStream("caminho/para/musica.mp3")`: Carrega uma música (ideal para trilhas longas).
- `UpdateMusicStream()`, `PlayMusicStream()`, `StopMusicStream()`.

### Texto

Além da `DrawText` simples, a Raylib oferece suporte a fontes TTF.

- `LoadFont("caminho/para/fonte.ttf")`: Carrega uma fonte TrueType.
- `DrawTextEx(Font fonte, string texto, Vector2 posicao, float tamanhoFonte, float espaçamento, Color cor)`: Desenha texto com uma fonte carregada, oferecendo mais controle .

## 5. Particularidades do Raylib-cs em C#

Ao vir de exemplos em C, é importante notar algumas adaptações que o bindings faz para tornar a experiência mais natural em C# :

- **Enums no lugar de inteiros:** Funções que esperavam constantes numéricas agora usam `enum`s fortemente tipados, como `KeyboardKey` e `MouseButton`. Isso previne erros e melhora a autocompletar do código.
- **Cores como membros estáticos:** As cores são acessadas como membros estáticos da struct `Color`. Em vez de `RAYWHITE` (C), usa-se `Color.RayWhite`. Em vez de `RED`, usa-se `Color.Red` .
- **`string.Format` em vez de `TextFormat`:** A função `TextFormat` do C não é necessária. Você pode usar a interpolação de strings do C# (`$"Pontuação: {pontuacao}"`) ou `string.Format` diretamente.
- **Construtores e sobrecarga de operadores:** As structs matemáticas como `Vector2`, `Vector3` e `Rectangle` vêm com construtores e operadores sobrecarregados (como `+`, `-`, `*`), permitindo um código mais intuitivo .

## 6. Exemplos Práticos

### Desenhando e Movimentando uma Forma

Este exemplo mostra um quadrado que se move para a direita sozinho e reseta ao sair da tela.

```csharp
using Raylib_cs;

namespace Movimento;
class Program
{
    public static void Main()
    {
        const int larguraTela = 800;
        const int alturaTela = 600;
        Raylib.InitWindow(larguraTela, alturaTela, "Movimento em Raylib-cs");

        // Posição inicial do quadrado
        Vector2 posicaoQuadrado = new Vector2(100, alturaTela / 2);
        const float velocidade = 200.0f; // pixels por segundo
        const float ladoQuadrado = 50.0f;

        Raylib.SetTargetFPS(60); // Limita a 60 frames por segundo

        while (!Raylib.WindowShouldClose())
        {
            // Calcula o delta time (tempo desde o último frame)
            float dt = Raylib.GetFrameTime();

            // Atualiza a posição (move para a direita)
            posicaoQuadrado.X += velocidade * dt;

            // Reset da posição se sair da tela
            if (posicaoQuadrado.X > larguraTela)
            {
                posicaoQuadrado.X = -ladoQuadrado;
            }

            // Desenho
            Raylib.BeginDrawing();
            Raylib.ClearBackground(Color.DarkGray);

            Raylib.DrawRectangleV(posicaoQuadrado, new Vector2(ladoQuadrado, ladoQuadrado), Color.Gold);

            Raylib.EndDrawing();
        }

        Raylib.CloseWindow();
    }
}
```

### Carregando e Desenhando uma Textura (Imagem)

Para este exemplo, certifique-se de ter uma imagem (por exemplo, `player.png`) na mesma pasta do executável (dentro de `bin/Debug/netX.0/`).

```csharp
using Raylib_cs;

namespace Texturas;
class Program
{
    public static void Main()
    {
        Raylib.InitWindow(800, 600, "Desenhando Imagem");

        // Carrega a textura do arquivo
        Texture2D textura = Raylib.LoadTexture("player.png");
        Vector2 posicao = new Vector2(100, 100);

        while (!Raylib.WindowShouldClose())
        {
            Raylib.BeginDrawing();
            Raylib.ClearBackground(Color.RayWhite);

            // Desenha a textura na posição (100, 100)
            Raylib.DrawTextureV(textura, posicao, Color.White);

            Raylib.EndDrawing();
        }

        // É crucial descarregar a textura da memória ao final
        Raylib.UnloadTexture(textura);
        Raylib.CloseWindow();
    }
}
```

### Tocando um Som Simples

```csharp
using Raylib_cs;

namespace Audio;
class Program
{
    public static void Main()
    {
        Raylib.InitWindow(800, 600, "Exemplo de Áudio");
        Raylib.InitAudioDevice(); // Inicializa o dispositivo de áudio

        Sound somPulo = Raylib.LoadSound("pulo.wav"); // Carrega o som

        while (!Raylib.WindowShouldClose())
        {
            // Se a barra de espaço for pressionada, toca o som
            if (Raylib.IsKeyPressed(KeyboardKey.Space))
            {
                Raylib.PlaySound(somPulo);
            }

            Raylib.BeginDrawing();
            Raylib.ClearBackground(Color.Blue);
            Raylib.DrawText("Pressione ESPACO para tocar o som", 50, 300, 20, Color.LightGray);
            Raylib.EndDrawing();
        }

        Raylib.UnloadSound(somPulo); // Descarrega o som
        Raylib.CloseAudioDevice();   // Fecha o dispositivo de áudio
        Raylib.CloseWindow();
    }
}
