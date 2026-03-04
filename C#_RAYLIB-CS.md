# Manual Completo da Sintaxe do Raylib-cs

## 1. Introdução

**Raylib-cs** é um conjunto de bindings (ligações) que permitem usar a poderosa e simples biblioteca gráfica **Raylib** (escrita em C) diretamente em C# . Isso significa que você pode criar jogos e aplicações multimídia com a simplicidade da Raylib, aproveitando todos os recursos e a sintaxe moderna do C# e do ecossistema .NET.

Este manual foi criado para servir como um guia de consulta rápida e referência para a sintaxe e as convenções adotadas pelo Raylib-cs.

## 2. Configuração do Ambiente

### 2.1. Instalação via NuGet (Recomendado)

A maneira mais fácil de começar é adicionar o pacote NuGet ao seu projeto .NET.

1.  **Crie um novo projeto Console**:
    ```bash
    dotnet new console -n MeuJogoRaylib
    cd MeuJogoRaylib
    ```

2.  **Adicione o pacote Raylib-cs**:
    ```bash
    dotnet add package Raylib-cs
    ```
    Este comando baixa e adiciona as bindings ao seu projeto. As bibliotecas nativas (`raylib.dll` para Windows, `libraylib.so` para Linux, etc.) são incluídas automaticamente pelo pacote .

### 2.2. Primeiro Programa: "Hello, World"

Após a instalação, abra o arquivo `Program.cs` e substitua o conteúdo pelo código abaixo para ver a Raylib em ação.

```csharp
using Raylib_cs; // 1. Namespace principal

namespace MeuJogoRaylib;

class Program
{
    public static void Main()
    {
        // 2. Inicializa a janela
        Raylib.InitWindow(800, 480, "Olá, Mundo com Raylib-cs!");

        // 3. Loop principal do jogo
        while (!Raylib.WindowShouldClose()) // Verifica se o usuário mandou fechar a janela (ESC ou clique no X)
        {
            // 4. Começa a desenhar
            Raylib.BeginDrawing();
            Raylib.ClearBackground(Color.White); // Limpa a tela com a cor branca

            // 5. Desenha um texto
            Raylib.DrawText("Hello, world!", 12, 12, 20, Color.Black);

            // 6. Termina o desenho
            Raylib.EndDrawing();
        }

        // 7. Fecha a janela e libera recursos
        Raylib.CloseWindow();
    }
}
```

**Execute o projeto:**
```bash
dotnet run
```

Uma janela com o texto "Hello, world!" deve aparecer .

## 3. Estrutura Básica de um Programa

Como visto no exemplo acima, todo programa com Raylib-cs segue uma estrutura básica:

1.  **Importação do Namespace**: `using Raylib_cs;` .
2.  **Inicialização**: `Raylib.InitWindow()` cria a janela. Outras inicializações, como áudio (`Raylib.InitAudioDevice()`), podem ser feitas aqui .
3.  **Game Loop**: O loop `while` que mantém o programa rodando. Ele só termina quando `Raylib.WindowShouldClose()` retorna `true` .
4.  **Desenho na Tela**:
    - `Raylib.BeginDrawing()` e `Raylib.EndDrawing()` delimitam o bloco de desenho.
    - `Raylib.ClearBackground()` redefine a cor de fundo da tela.
    - Funções como `DrawText()`, `DrawCircle()`, `DrawTexture()` desenham na tela .
5.  **Encerramento**: `Raylib.CloseWindow()` finaliza a janela. Se o áudio foi iniciado, `Raylib.CloseAudioDevice()` deve ser chamado.

## 4. Módulos e Sintaxe por Categoria

A API do Raylib-cs é uma tradução direta da API original em C, mas seguindo as convenções do C#.

### 4.1. Janela e Sistema (`Raylib`)

| Função | Descrição |
| :--- | :--- |
| `Raylib.InitWindow(int width, int height, string title)` | Inicializa a janela. |
| `Raylib.WindowShouldClose()` | Retorna `true` se a janela deve fechar. |
| `Raylib.CloseWindow()` | Fecha a janela. |
| `Raylib.BeginDrawing()` | Inicia o contexto de desenho. |
| `Raylib.EndDrawing()` | Finaliza o contexto de desenho. |
| `Raylib.ClearBackground(Color color)` | Limpa a tela com uma cor. |
| `Raylib.SetTargetFPS(int fps)` | Define um limite de quadros por segundo. |
| `Raylib.GetFrameTime()` | Retorna o tempo (em segundos) entre o frame atual e o anterior. |

### 4.2. Formas Geométricas 2D

| Função | Descrição |
| :--- | :--- |
| `Raylib.DrawPixel(int x, int y, Color color)` | Desenha um pixel. |
| `Raylib.DrawLine(int x1, int y1, int x2, int y2, Color color)` | Desenha uma linha. |
| `Raylib.DrawCircle(int x, int y, float radius, Color color)` | Desenha um círculo preenchido. |
| `Raylib.DrawCircleV(Vector2 center, float radius, Color color)` | Desenha um círculo usando `Vector2` para o centro. |
| `Raylib.DrawCircleLines(int x, int y, float radius, Color color)` | Desenha o contorno de um círculo. |
| `Raylib.DrawRectangle(int x, int y, int width, int height, Color color)` | Desenha um retângulo preenchido. |
| `Raylib.DrawRectangleV(Vector2 position, Vector2 size, Color color)` | Desenha um retângulo usando `Vector2`. |
| `Raylib.DrawRectangleRec(Rectangle rec, Color color)` | Desenha um retângulo usando a struct `Rectangle`. |
| `Raylib.DrawRectangleLines(int x, int y, int width, int height, Color color)` | Desenha o contorno de um retângulo. |

### 4.3. Texto

| Função | Descrição |
| :--- | :--- |
| `Raylib.DrawText(string text, int x, int y, int fontSize, Color color)` | Desenha um texto usando a fonte padrão. |
| `Raylib.MeasureText(string text, int fontSize)` | Mede a largura de um texto na fonte padrão. |
| **Fonte Personalizada** | |
| `Font font = Raylib.LoadFont("caminho/para/fonte.ttf")` | Carrega uma fonte de um arquivo TTF. |
| `Raylib.DrawTextEx(Font font, string text, Vector2 position, float fontSize, float spacing, Color color)` | Desenha um texto com uma fonte carregada. |
| `Raylib.UnloadFont(Font font)` | Descarrega a fonte da memória. |

### 4.4. Texturas e Imagens

| Etapa | Função | Descrição |
| :--- | :--- | :--- |
| **Carregar** | `Texture2D texture = Raylib.LoadTexture("caminho/para/imagem.png")` | Carrega uma textura da GPU a partir de um arquivo de imagem. |
| | `Image image = Raylib.LoadImage("caminho/para/imagem.png")` | Carrega uma imagem para a CPU (para manipulação de pixels). |
| | `Texture2D texture = Raylib.LoadTextureFromImage(Image image)` | Cria uma textura na GPU a partir de uma imagem na CPU. |
| **Desenhar** | `Raylib.DrawTexture(Texture2D texture, int x, int y, Color tint)` | Desenha a textura em sua resolução original. O parâmetro `tint` permite aplicar uma cor (use `Color.White` para a cor original). |
| | `Raylib.DrawTextureV(Texture2D texture, Vector2 position, Color tint)` | Usa `Vector2` para a posição. |
| | `Raylib.DrawTextureEx(Texture2D texture, Vector2 position, float rotation, float scale, Color tint)` | Oferece mais controle com rotação e escala. |
| | `Raylib.DrawTextureRec(Texture2D texture, Rectangle sourceRect, Vector2 position, Color tint)` | Desenha apenas uma parte (um recorte) da textura. |
| **Descarregar**| `Raylib.UnloadTexture(Texture2D texture)` | Libera a textura da memória da GPU. |
| | `Raylib.UnloadImage(Image image)` | Libera a imagem da memória da CPU. |

### 4.5. Entrada (Input)

| Categoria | Função | Descrição |
| :--- | :--- | :--- |
| **Teclado** | `Raylib.IsKeyPressed(KeyboardKey key)` | Retorna `true` apenas no frame em que a tecla foi pressionada. |
| | `Raylib.IsKeyDown(KeyboardKey key)` | Retorna `true` enquanto a tecla estiver sendo segurada. |
| | `Raylib.IsKeyReleased(KeyboardKey key)` | Retorna `true` apenas no frame em que a tecla foi solta. |
| | *Exemplo*: `Raylib.IsKeyDown(KeyboardKey.KEY_SPACE)` |
| **Mouse** | `Raylib.IsMouseButtonPressed(MouseButton button)` | Retorna `true` apenas no frame em que o botão foi pressionado. |
| | `Raylib.GetMousePosition()` | Retorna um `Vector2` com a posição X e Y do cursor. |
| | `Raylib.GetMouseDelta()` | Retorna um `Vector2` com o movimento do mouse desde o último frame. |
| | *Exemplo*: `Raylib.IsMouseButtonDown(MouseButton.MOUSE_BUTTON_LEFT)` |
| **Gamepad** | `Raylib.IsGamepadAvailable(int gamepad)` | Verifica se um gamepad está conectado. |
| | `Raylib.GetGamepadAxisMovement(int gamepad, GamepadAxis axis)` | Retorna o valor de um eixo analógico (entre -1 e 1). |
| | `Raylib.IsGamepadButtonPressed(int gamepad, GamepadButton button)` | Verifica se um botão do gamepad foi pressionado. |

### 4.6. Áudio

Para usar áudio, é necessário iniciar e fechar o dispositivo de áudio.

```csharp
Raylib.InitAudioDevice(); // Inicializa o subsistema de áudio
// ...
Music music = Raylib.LoadMusicStream("caminho/para/musica.ogg");
Sound fx = Raylib.LoadSound("caminho/para/som.wav");

Raylib.PlayMusicStream(music); // Começa a tocar a música
Raylib.PlaySound(fx);          // Toca o efeito sonoro

// Dentro do game loop, é preciso atualizar o stream de música
while (!Raylib.WindowShouldClose())
{
    Raylib.UpdateMusicStream(music); // Mantém a música tocando
    // ... resto da lógica e desenho
}

// Ao finalizar
Raylib.UnloadMusicStream(music);
Raylib.UnloadSound(fx);
Raylib.CloseAudioDevice(); // Fecha o dispositivo de áudio
```

### 4.7. Matemática e Structs

Raylib-cs faz uso intenso de structs para representar vetores, cores e retângulos.

| Struct | Campos/Propriedades | Descrição |
| :--- | :--- | :--- |
| **`System.Numerics.Vector2`** | `X`, `Y` | Usado para posições, tamanhos, velocidade, etc. |
| **`Raylib_cs.Color`** | `R`, `G`, `B`, `A` (bytes) | Representa uma cor. Acesse cores pré-definidas via `Color.Nome`. Ex: `Color.Red`, `Color.Blue`, `Color.White`, `Color.Black` . |
| **`Raylib_cs.Rectangle`** | `X`, `Y`, `Width`, `Height` | Usado para representar áreas, colisões e recortes de textura. |
| **`System.Numerics.Vector3`** | `X`, `Y`, `Z` | Usado para posições 3D. |
| **`Raylib_cs.Camera2D`** | `Offset`, `Target`, `Rotation`, `Zoom` | Define a câmera para jogos 2D. |
| **`Raylib_cs.Camera3D`** | `Position`, `Target`, `Up`, `Fovy`, `Projection` | Define a câmera para jogos 3D. |

> **Nota sobre Cores:** No Raylib original, as cores são constantes em MAIÚSCULAS. No Raylib-cs, elas são propriedades estáticas da struct `Color`, seguindo a convenção PascalCase do C# (ex: `Color.RED` no original se torna `Color.Red`) .

## 5. Particularidades do Raylib-cs (C#)

- **Métodos Estáticos**: Todas as funções da Raylib estão dentro da classe estática `Raylib`. Você as chama com `Raylib.NomeDaFuncao()` .
- **Enums**: Onde a API original usa inteiros ou constantes, o Raylib-cs usa enums para segurança e clareza, como `KeyboardKey.KEY_A` e `MouseButton.MOUSE_BUTTON_LEFT` .
- **Cores**: Conforme mencionado, as cores pré-definidas são acessadas como membros estáticos da struct `Color` (`Color.NomeDaCor`), com a primeira letra maiúscula .
- **Strings**: As funções que recebem texto (`string`) funcionam naturalmente em C#. O binding cuida da conversão para os ponteiros de char esperados pela biblioteca nativa .
- **Math**: O Raylib-cs incentiva o uso das structs `Vector2` e `Vector3` do namespace `System.Numerics`, que já possuem diversos operadores e métodos úteis sobrecarregados. Você pode fazer operações como `Vector2 velocidade = posicao2 - posicao1;` ou `Vector2 centro = posicao + new Vector2(largura/2, altura/2);` .

## 6. Exemplo Completo: Movendo um Retângulo

Este exemplo junta vários conceitos: entrada do teclado, desenho de formas e uso do `Vector2`.

```csharp
using Raylib_cs;
using System.Numerics; // Para Vector2

namespace ExemploMovimento;

class Program
{
    public static void Main()
    {
        const int larguraTela = 800;
        const int alturaTela = 600;

        Raylib.InitWindow(larguraTela, alturaTela, "Movendo Retângulo");
        Raylib.SetTargetFPS(60); // Limita a 60 FPS

        Vector2 posicaoRetangulo = new Vector2(larguraTela / 2, alturaTela / 2);
        const float velocidade = 5.0f;

        while (!Raylib.WindowShouldClose())
        {
            // --- Lógica de Atualização ---
            if (Raylib.IsKeyDown(KeyboardKey.KEY_RIGHT) || Raylib.IsKeyDown(KeyboardKey.KEY_D))
                posicaoRetangulo.X += velocidade;
            if (Raylib.IsKeyDown(KeyboardKey.KEY_LEFT) || Raylib.IsKeyDown(KeyboardKey.KEY_A))
                posicaoRetangulo.X -= velocidade;
            if (Raylib.IsKeyDown(KeyboardKey.KEY_DOWN) || Raylib.IsKeyDown(KeyboardKey.KEY_S))
                posicaoRetangulo.Y += velocidade;
            if (Raylib.IsKeyDown(KeyboardKey.KEY_UP) || Raylib.IsKeyDown(KeyboardKey.KEY_W))
                posicaoRetangulo.Y -= velocidade;

            // --- Desenho ---
            Raylib.BeginDrawing();
            Raylib.ClearBackground(Color.RayWhite);

            // Desenha o retângulo na posição calculada
            Raylib.DrawRectangleV(posicaoRetangulo, new Vector2(50, 50), Color.Maroon);

            Raylib.EndDrawing();
        }

        Raylib.CloseWindow();
    }
}
```

## 7. Recursos Adicionais

- **Documentação Oficial da Raylib**: [https://www.raylib.com/](https://www.raylib.com/)
- **Repositório do Raylib-cs no GitHub**: [https://github.com/ChrisDill/Raylib-cs](https://github.com/ChrisDill/Raylib-cs) 
- **Exemplos de Código**: O repositório oficial do Raylib-cs costuma ter uma pasta `Examples` com diversos casos de uso .

