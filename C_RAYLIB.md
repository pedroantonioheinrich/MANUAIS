# 📚 Manual Completo: Desenvolvimento com Raylib em C

## 🎯 Introdução

Raylib é uma biblioteca simples e fácil de usar para desenvolvimento de jogos e multimídia. Criada por Ramon Santamaria, ela foi projetada para ser intuitiva, mesmo para iniciantes em programação.

### Por que Raylib?
- ✅ **Simples**: API consistente e fácil de aprender
- ✅ **Poderosa**: Suporte a 2D, 3D, áudio, shaders
- ✅ **Portável**: Windows, Linux, macOS, Android, Web
- ✅ **Sem dependências**: Tudo em um único pacote

## 🔧 Instalação

### Windows (com MinGW)
```bash
# Baixe o arquivo da raylib do site oficial
# Extraia para C:raylib
# Configure as variáveis de ambiente
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install libraylib-dev
```

### Compilando um programa
```bash
gcc meu_jogo.c -o meu_jogo -lraylib -lm
```

## 📖 Fundamentos Básicos

### 1️⃣ Primeiro Programa - Janela Vazia

```c
#include "raylib.h"

int main(void)
{
    // Inicialização
    const int screenWidth = 800;
    const int screenHeight = 450;
    
    InitWindow(screenWidth, screenHeight, "Meu Primeiro Jogo!");
    
    SetTargetFPS(60); // Limita a 60 frames por segundo
    
    // Loop principal do jogo
    while (!WindowShouldClose()) // Enquanto não fechar a janela
    {
        // Desenho
        BeginDrawing();
        
        ClearBackground(RAYWHITE); // Limpa a tela com cor branca
        
        DrawText("Olá, Raylib!", 190, 200, 40, BLACK);
        
        EndDrawing();
    }
    
    // Limpeza
    CloseWindow();
    
    return 0;
}
```

### 2️⃣ Estrutura Básica de um Jogo

```c
#include "raylib.h"

int main(void)
{
    // Configuração inicial
    InitWindow(800, 600, "Estrutura de Jogo");
    InitAudioDevice(); // Inicializa áudio
    
    // Carregar recursos (imagens, sons, fontes)
    Texture2D playerTexture = LoadTexture("player.png");
    Sound jumpSound = LoadSound("jump.wav");
    
    SetTargetFPS(60);
    
    // Game loop
    while (!WindowShouldClose())
    {
        // 1. UPDATE - Atualizar lógica do jogo
        float deltaTime = GetFrameTime(); // Tempo desde último frame
        
        if (IsKeyPressed(KEY_SPACE))
        {
            PlaySound(jumpSound); // Tocar som
        }
        
        // 2. DRAW - Renderizar
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        // Desenhar textura
        DrawTexture(playerTexture, 100, 100, WHITE);
        
        DrawFPS(10, 10); // Mostrar FPS (útil para debug)
        
        EndDrawing();
    }
    
    // Descarregar recursos
    UnloadTexture(playerTexture);
    UnloadSound(jumpSound);
    
    CloseAudioDevice();
    CloseWindow();
    
    return 0;
}
```

## 🎨 Desenhando Formas Básicas

### 2D Primitivas

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Formas Básicas");
    SetTargetFPS(60);
    
    // Posições dos objetos
    Vector2 circlePos = { 400, 300 };
    float circleRadius = 50;
    
    Rectangle rect = { 100, 100, 200, 150 };
    float rotation = 0;
    
    while (!WindowShouldClose())
    {
        // Atualizar rotação
        rotation += 0.5f;
        
        BeginDrawing();
        ClearBackground(DARKGRAY);
        
        // Linhas
        DrawLine(10, 10, 300, 300, RED);
        DrawLineEx((Vector2){ 20, 20 }, (Vector2){ 350, 50 }, 5, GREEN); // Linha grossa
        
        // Círculos
        DrawCircle(circlePos.x, circlePos.y, circleRadius, BLUE);
        DrawCircleGradient(200, 400, 60, RED, YELLOW); // Gradiente
        DrawCircleLines(600, 400, 80, WHITE); // Apenas contorno
        
        // Retângulos
        DrawRectangle(50, 400, 100, 80, PURPLE);
        DrawRectangleRec(rect, ORANGE); // Usando Rectangle struct
        DrawRectanglePro(rect, (Vector2){ 100, 75 }, rotation, GREEN); // Rotacionado
        DrawRectangleLines(500, 200, 150, 100, WHITE); // Apenas contorno
        
        // Triângulos
        DrawTriangle(
            (Vector2){ 600, 500 }, 
            (Vector2){ 650, 550 }, 
            (Vector2){ 550, 550 }, 
            PINK
        );
        
        // Polígonos
        DrawPoly((Vector2){ 700, 150 }, 6, 80, rotation, LIME); // Hexágono
        
        EndDrawing();
    }
    
    CloseWindow();
    return 0;
}
```

## 🎮 Input do Usuário

### Teclado, Mouse e Gamepad

```c
#include "raylib.h"
#include <stdio.h>

int main(void)
{
    InitWindow(800, 600, "Sistema de Input");
    SetTargetFPS(60);
    
    Vector2 playerPos = { 400, 300 };
    Vector2 mousePos = { 0, 0 };
    float speed = 200;
    
    // Para detecção de duplo clique
    double lastClickTime = 0;
    
    while (!WindowShouldClose())
    {
        float dt = GetFrameTime();
        
        // ===== TECLADO =====
        // Movimento suave (pressionado)
        if (IsKeyDown(KEY_RIGHT)) playerPos.x += speed * dt;
        if (IsKeyDown(KEY_LEFT)) playerPos.x -= speed * dt;
        if (IsKeyDown(KEY_UP)) playerPos.y -= speed * dt;
        if (IsKeyDown(KEY_DOWN)) playerPos.y += speed * dt;
        
        // Apenas quando pressiona (uma vez)
        if (IsKeyPressed(KEY_SPACE))
        {
            printf("Espaço pressionado!\n");
        }
        
        // Verificar teclas específicas
        if (IsKeyPressed(KEY_ENTER) && IsKeyDown(KEY_LEFT_CONTROL))
        {
            printf("Ctrl+Enter pressionado!\n");
            ToggleFullscreen(); // Alternar tela cheia
        }
        
        // ===== MOUSE =====
        // Posição do mouse
        mousePos = GetMousePosition();
        
        // Botões do mouse
        if (IsMouseButtonPressed(MOUSE_LEFT_BUTTON))
        {
            printf("Clique esquerdo em (%.0f, %.0f)\n", mousePos.x, mousePos.y);
        }
        
        if (IsMouseButtonDown(MOUSE_RIGHT_BUTTON))
        {
            DrawCircle(mousePos.x, mousePos.y, 20, RED); // Desenha enquanto segura
        }
        
        // Roda do mouse
        float wheelMove = GetMouseWheelMove();
        if (wheelMove != 0)
        {
            printf("Roda: %.2f\n", wheelMove);
        }
        
        // ===== GAMEPAD =====
        if (IsGamepadAvailable(0)) // Verifica se gamepad 0 está conectado
        {
            // Movimento com analógico
            float moveX = GetGamepadAxisMovement(0, GAMEPAD_AXIS_LEFT_X);
            float moveY = GetGamepadAxisMovement(0, GAMEPAD_AXIS_LEFT_Y);
            
            playerPos.x += moveX * speed * dt * 2;
            playerPos.y += moveY * speed * dt * 2;
            
            // Botões do gamepad
            if (IsGamepadButtonPressed(0, GAMEPAD_BUTTON_RIGHT_FACE_DOWN))
            {
                printf("Botão A pressionado!\n");
            }
        }
        
        // ===== TOQUE (mobile) =====
        if (IsGestureDetected(GESTURE_TAP))
        {
            Vector2 touchPos = GetTouchPosition(0);
            printf("Toque em (%.0f, %.0f)\n", touchPos.x, touchPos.y);
        }
        
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        // Desenhar player
        DrawCircleV(playerPos, 30, MAROON);
        
        // Informações na tela
        DrawText("Use as setas para mover o círculo", 10, 10, 20, DARKGRAY);
        
        char mouseInfo[50];
        sprintf(mouseInfo, "Mouse: (%.0f, %.0f)", mousePos.x, mousePos.y);
        DrawText(mouseInfo, 10, 40, 20, DARKGRAY);
        
        if (IsGamepadAvailable(0))
        {
            DrawText("Gamepad conectado!", 10, 70, 20, GREEN);
        }
        
        EndDrawing();
    }
    
    CloseWindow();
    return 0;
}
```

## 🖼️ Trabalhando com Texturas e Imagens

### Carregamento e Manipulação de Imagens

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Texturas e Imagens");
    InitAudioDevice();
    
    // Carregar imagem como textura (para GPU)
    Texture2D player = LoadTexture("resources/player.png");
    Texture2D background = LoadTexture("resources/background.png");
    
    // Carregar imagem para manipulação CPU
    Image originalImage = LoadImage("resources/character.png");
    
    // Manipular imagem
    ImageResize(&originalImage, 100, 100); // Redimensionar
    ImageColorInvert(&originalImage); // Inverter cores
    ImageFlipVertical(&originalImage); // Inverter verticalmente
    
    // Converter imagem manipulada para textura
    Texture2D processedTexture = LoadTextureFromImage(originalImage);
    
    // Descarregar imagem da CPU (já temos na GPU)
    UnloadImage(originalImage);
    
    // Carregar sprite sheet (várias imagens em uma)
    Image spriteSheetImage = LoadImage("resources/spritesheet.png");
    Texture2D spriteSheet = LoadTextureFromImage(spriteSheetImage);
    
    // Definir frames do sprite sheet (supondo 4x4 grid)
    Rectangle frameRec = { 0, 0, 
                          (float)spriteSheet.width / 4, 
                          (float)spriteSheet.height / 4 };
    
    int currentFrame = 0;
    int framesCounter = 0;
    
    SetTargetFPS(60);
    
    while (!WindowShouldClose())
    {
        // Animação do sprite
        framesCounter++;
        if (framesCounter >= 10) // Troca a cada 10 frames
        {
            framesCounter = 0;
            currentFrame++;
            if (currentFrame > 3) currentFrame = 0;
            
            // Atualizar retângulo do frame atual
            frameRec.x = (float)currentFrame * frameRec.width;
        }
        
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        // Desenhar background
        DrawTexture(background, 0, 0, WHITE);
        
        // Desenhar textura normal
        DrawTexture(player, 100, 200, WHITE);
        
        // Desenhar textura com rotação
        DrawTextureEx(player, (Vector2){ 300, 200 }, 45, 1.5f, WHITE);
        
        // Desenhar textura processada
        DrawTexture(processedTexture, 500, 200, WHITE);
        
        // Desenhar frame atual do sprite sheet
        DrawTextureRec(spriteSheet, frameRec, (Vector2){ 200, 400 }, WHITE);
        
        // Desenhar texto com fonte personalizada
        DrawText("Texturas e Sprites", 300, 50, 30, DARKBLUE);
        
        EndDrawing();
    }
    
    // Limpeza
    UnloadTexture(player);
    UnloadTexture(background);
    UnloadTexture(processedTexture);
    UnloadTexture(spriteSheet);
    
    CloseWindow();
    return 0;
}
```

## 🔊 Áudio

### Sons e Música

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Sistema de Áudio");
    InitAudioDevice(); // IMPORTANTE: Inicializar áudio primeiro
    
    // Carregar efeitos sonoros (WAV)
    Sound jumpSound = LoadSound("resources/jump.wav");
    Sound coinSound = LoadSound("resources/coin.wav");
    Sound explosionSound = LoadSound("resources/explosion.wav");
    
    // Carregar música (OGG/MP3) - streaming, não carrega completamente
    Music backgroundMusic = LoadMusicStream("resources/background.ogg");
    
    // Configurar música
    PlayMusicStream(backgroundMusic);
    SetMusicVolume(backgroundMusic, 0.5f); // 50% volume
    
    // Configurar sons
    SetSoundVolume(jumpSound, 0.8f);
    SetSoundPitch(jumpSound, 1.2f); // Tom mais agudo
    
    float timePlayed = 0.0f;
    
    SetTargetFPS(60);
    
    while (!WindowShouldClose())
    {
        // Atualizar streaming de música (OBRIGATÓRIO)
        UpdateMusicStream(backgroundMusic);
        
        // Obter tempo da música
        timePlayed = GetMusicTimePlayed(backgroundMusic) / GetMusicTimeLength(backgroundMusic);
        
        // Input para sons
        if (IsKeyPressed(KEY_J)) PlaySound(jumpSound);
        if (IsKeyPressed(KEY_C)) PlaySound(coinSound);
        if (IsKeyPressed(KEY_E)) PlaySound(explosionSound);
        
        // Controles da música
        if (IsKeyPressed(KEY_P))
        {
            if (IsMusicStreamPlaying(backgroundMusic))
                PauseMusicStream(backgroundMusic);
            else
                ResumeMusicStream(backgroundMusic);
        }
        
        if (IsKeyPressed(KEY_S)) StopMusicStream(backgroundMusic);
        if (IsKeyPressed(KEY_R)) PlayMusicStream(backgroundMusic);
        
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        // Instruções
        DrawText("Pressione:", 50, 50, 20, DARKGRAY);
        DrawText("J - Pular", 50, 80, 20, DARKGRAY);
        DrawText("C - Moeda", 50, 110, 20, DARKGRAY);
        DrawText("E - Explosão", 50, 140, 20, DARKGRAY);
        DrawText("P - Pausar/Continuar música", 50, 170, 20, DARKGRAY);
        DrawText("S - Parar música", 50, 200, 20, DARKGRAY);
        DrawText("R - Recomeçar música", 50, 230, 20, DARKGRAY);
        
        // Barra de progresso da música
        DrawRectangle(50, 300, 500, 20, LIGHTGRAY);
        DrawRectangle(50, 300, (int)(500 * timePlayed), 20, MAROON);
        DrawText("Progresso da Música", 50, 330, 20, DARKGRAY);
        
        EndDrawing();
    }
    
    // Limpeza
    UnloadSound(jumpSound);
    UnloadSound(coinSound);
    UnloadSound(explosionSound);
    UnloadMusicStream(backgroundMusic);
    
    CloseAudioDevice();
    CloseWindow();
    return 0;
}
```

## 📝 Fontes e Texto

### Renderização de Texto Avançada

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Sistema de Texto");
    
    // Fontes diferentes
    Font defaultFont = GetFontDefault(); // Fonte padrão
    Font customFont = LoadFont("resources/kenney_font.ttf");
    Font pixelFont = LoadFontEx("resources/pixel.ttf", 32, 0, 250);
    
    // Para animação de texto
    float textCounter = 0;
    const char* typingText = "Digitando... ";
    int typingLength = 0;
    
    SetTargetFPS(60);
    
    while (!WindowShouldClose())
    {
        // Animação de digitação
        textCounter += GetFrameTime() * 10;
        typingLength = (int)textCounter % strlen(typingText);
        
        BeginDrawing();
        ClearBackground(DARKPURPLE);
        
        // Texto básico
        DrawText("Texto Padrão (raylib)", 50, 50, 20, WHITE);
        
        // Texto com fonte customizada
        DrawTextEx(customFont, "Fonte Customizada", 
                  (Vector2){ 50, 100 }, 20, 2, YELLOW);
        
        // Texto com sombra (desenhar duas vezes)
        DrawText("Texto com Sombra", 52, 152, 30, DARKGRAY); // Sombra
        DrawText("Texto com Sombra", 50, 150, 30, GREEN);   // Texto
        
        // Texto centralizado
        const char* centeredText = "Texto Centralizado";
        int fontSize = 30;
        int textWidth = MeasureText(centeredText, fontSize);
        DrawText(centeredText, 400 - textWidth/2, 250, fontSize, WHITE);
        
        // Texto dentro de uma caixa
        Rectangle textBox = { 300, 320, 200, 100 };
        DrawRectangleRec(textBox, BLUE);
        DrawRectangleLinesEx(textBox, 2, WHITE);
        DrawTextRec(customFont, "Texto longo que deve quebrar automaticamente dentro da caixa", 
                   textBox, 20, 2, true, WHITE);
        
        // Texto codificado (UTF-8)
        DrawText("Caracteres especiais: á é í ó ú ç ñ", 50, 450, 20, WHITE);
        
        // Animação de digitação
        char typing[50];
        strncpy(typing, typingText, typingLength);
        typing[typingLength] = '\0';
        DrawText(TextFormat("Digitando: %s", typing), 50, 500, 20, WHITE);
        
        // Texto formatado
        DrawText(TextFormat("FPS: %i", GetFPS()), 700, 10, 20, GREEN);
        
        EndDrawing();
    }
    
    // Limpeza
    UnloadFont(customFont);
    UnloadFont(pixelFont);
    
    CloseWindow();
    return 0;
}
```

## 🎲 Sistema de Partículas

### Efeitos Visuais

```c
#include "raylib.h"
#include <stdlib.h>
#include <math.h>

#define MAX_PARTICLES 100

typedef struct {
    Vector2 position;
    Vector2 velocity;
    Color color;
    float radius;
    float life;
    bool active;
} Particle;

void InitParticle(Particle *p, Vector2 pos)
{
    p->position = pos;
    p->velocity.x = (GetRandomValue(-100, 100) / 10.0f);
    p->velocity.y = (GetRandomValue(-200, 0) / 10.0f);
    p->color = (Color){ 
        GetRandomValue(200, 255), 
        GetRandomValue(100, 200), 
        GetRandomValue(0, 100), 
        255 
    };
    p->radius = GetRandomValue(2, 8);
    p->life = 1.0f;
    p->active = true;
}

void UpdateParticle(Particle *p, float dt)
{
    if (!p->active) return;
    
    // Aplicar gravidade
    p->velocity.y += 500 * dt;
    
    // Atualizar posição
    p->position.x += p->velocity.x * dt;
    p->position.y += p->velocity.y * dt;
    
    // Reduzir vida
    p->life -= dt * 0.5f;
    
    // Desativar se vida acabou
    if (p->life <= 0)
    {
        p->active = false;
    }
}

int main(void)
{
    InitWindow(800, 600, "Sistema de Partículas");
    SetTargetFPS(60);
    
    Particle particles[MAX_PARTICLES] = {0};
    Vector2 emitterPos = { 400, 300 };
    
    while (!WindowShouldClose())
    {
        float dt = GetFrameTime();
        
        // Emitir novas partículas
        if (IsMouseButtonDown(MOUSE_LEFT_BUTTON))
        {
            emitterPos = GetMousePosition();
            
            for (int i = 0; i < 5; i++) // Criar 5 partículas por frame
            {
                // Encontrar partícula inativa
                for (int j = 0; j < MAX_PARTICLES; j++)
                {
                    if (!particles[j].active)
                    {
                        InitParticle(&particles[j], emitterPos);
                        break;
                    }
                }
            }
        }
        
        // Atualizar partículas
        for (int i = 0; i < MAX_PARTICLES; i++)
        {
            if (particles[i].active)
            {
                UpdateParticle(&particles[i], dt);
            }
        }
        
        BeginDrawing();
        ClearBackground(BLACK);
        
        // Desenhar partículas
        for (int i = 0; i < MAX_PARTICLES; i++)
        {
            if (particles[i].active)
            {
                Color color = particles[i].color;
                color.a = (unsigned char)(particles[i].life * 255);
                
                DrawCircleV(particles[i].position, 
                          particles[i].radius * particles[i].life, 
                          color);
            }
        }
        
        // Desenhar emissor
        DrawCircleV(emitterPos, 10, RED);
        
        DrawText("Clique e arraste para criar partículas", 10, 10, 20, WHITE);
        DrawText(TextFormat("Partículas ativas: %i", MAX_PARTICLES), 10, 40, 20, WHITE);
        
        EndDrawing();
    }
    
    CloseWindow();
    return 0;
}
```

## 🎯 Colisões e Física Simples

### Detecção de Colisão

```c
#include "raylib.h"
#include <math.h>

int main(void)
{
    InitWindow(800, 600, "Sistema de Colisões");
    SetTargetFPS(60);
    
    // Jogador (círculo)
    Vector2 playerPos = { 400, 300 };
    float playerRadius = 30;
    
    // Obstáculos
    Rectangle walls[] = {
        { 100, 100, 150, 20 },  // Parede horizontal
        { 600, 200, 20, 200 },  // Parede vertical
        { 200, 400, 200, 20 },  // Plataforma
        { 500, 450, 150, 150 }  // Bloco grande
    };
    int wallCount = sizeof(walls) / sizeof(walls[0]);
    
    // Itens colecionáveis
    Vector2 items[] = {
        { 150, 150 },
        { 650, 300 },
        { 300, 450 }
    };
    bool itemActive[] = { true, true, true };
    int itemCount = 3;
    float itemRadius = 20;
    
    // Inimigo
    Vector2 enemyPos = { 500, 100 };
    float enemyRadius = 25;
    Vector2 enemyDir = { 1, 0 };
    float enemySpeed = 100;
    
    int score = 0;
    bool gameOver = false;
    
    while (!WindowShouldClose() && !gameOver)
    {
        float dt = GetFrameTime();
        
        // Movimento do jogador
        Vector2 newPos = playerPos;
        if (IsKeyDown(KEY_RIGHT)) newPos.x += 200 * dt;
        if (IsKeyDown(KEY_LEFT)) newPos.x -= 200 * dt;
        if (IsKeyDown(KEY_UP)) newPos.y -= 200 * dt;
        if (IsKeyDown(KEY_DOWN)) newPos.y += 200 * dt;
        
        // Verificar colisão com paredes antes de mover
        bool canMove = true;
        for (int i = 0; i < wallCount; i++)
        {
            if (CheckCollisionCircleRec(newPos, playerRadius, walls[i]))
            {
                canMove = false;
                break;
            }
        }
        
        // Mover se não houver colisão
        if (canMove)
        {
            playerPos = newPos;
        }
        
        // Colisão com itens
        for (int i = 0; i < itemCount; i++)
        {
            if (itemActive[i] && 
                CheckCollisionCircles(playerPos, playerRadius, items[i], itemRadius))
            {
                itemActive[i] = false;
                score++;
            }
        }
        
        // Movimento do inimigo (ping-pong)
        enemyPos.x += enemyDir.x * enemySpeed * dt;
        if (enemyPos.x > 700 || enemyPos.x < 100)
        {
            enemyDir.x *= -1;
        }
        
        // Colisão com inimigo
        if (CheckCollisionCircles(playerPos, playerRadius, enemyPos, enemyRadius))
        {
            gameOver = true;
        }
        
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        // Desenhar paredes
        for (int i = 0; i < wallCount; i++)
        {
            DrawRectangleRec(walls[i], DARKBROWN);
        }
        
        // Desenhar itens
        for (int i = 0; i < itemCount; i++)
        {
            if (itemActive[i])
            {
                DrawCircleV(items[i], itemRadius, GOLD);
                DrawCircleLines(items[i].x, items[i].y, itemRadius, DARKGRAY);
            }
        }
        
        // Desenhar inimigo
        DrawCircleV(enemyPos, enemyRadius, RED);
        DrawCircleLines(enemyPos.x, enemyPos.y, enemyRadius, MAROON);
        
        // Desenhar jogador
        DrawCircleV(playerPos, playerRadius, BLUE);
        DrawCircleLines(playerPos.x, playerPos.y, playerRadius, DARKBLUE);
        
        // Desenhar informações
        DrawText(TextFormat("Pontuação: %i/%i", score, itemCount), 10, 10, 20, DARKGRAY);
        
        // Verificar vitória
        if (score == itemCount)
        {
            DrawText("VOCÊ VENCEU!", 300, 250, 50, GREEN);
        }
        
        EndDrawing();
    }
    
    // Tela de game over
    if (gameOver)
    {
        while (!WindowShouldClose())
        {
            BeginDrawing();
            ClearBackground(RED);
            DrawText("GAME OVER", 250, 250, 80, BLACK);
            DrawText("Pressione ESC para sair", 250, 350, 30, BLACK);
            EndDrawing();
        }
    }
    
    CloseWindow();
    return 0;
}
```

## 📦 Modos 3D

### Renderização 3D Básica

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Raylib 3D - Básico");
    
    // Configuração da câmera 3D
    Camera3D camera = { 0 };
    camera.position = (Vector3){ 10.0f, 10.0f, 10.0f };
    camera.target = (Vector3){ 0.0f, 0.0f, 0.0f };
    camera.up = (Vector3){ 0.0f, 1.0f, 0.0f };
    camera.fovy = 45.0f;
    camera.projection = CAMERA_PERSPECTIVE;
    
    // Objetos
    Vector3 cubePos = { 0.0f, 0.0f, 0.0f };
    Vector3 spherePos = { 3.0f, 1.0f, 2.0f };
    
    SetTargetFPS(60);
    
    // Desenhar um grid para referência
    bool drawGrid3D = true;
    
    while (!WindowShouldClose())
    {
        // Atualizar câmera com controles
        UpdateCamera(&camera, CAMERA_ORBITAL);
        
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        BeginMode3D(camera);
        
        // Desenhar grid
        if (drawGrid3D)
        {
            DrawGrid(20, 1.0f);
        }
        
        // Cubo
        DrawCube(cubePos, 2.0f, 2.0f, 2.0f, RED);
        DrawCubeWires(cubePos, 2.0f, 2.0f, 2.0f, MAROON);
        
        // Esfera
        DrawSphere(spherePos, 1.0f, BLUE);
        DrawSphereWires(spherePos, 1.0f, 16, 16, DARKBLUE);
        
        // Plano (chão)
        DrawPlane((Vector3){ 0, -1, 0 }, (Vector2){ 20, 20 }, GREEN);
        
        // Cilindro
        DrawCylinder((Vector3){ -3, 0, -2 }, 1.0f, 1.0f, 3.0f, 16, ORANGE);
        
        // Cubo com textura
        DrawCubeTexture(LoadTexture("crate.png"), (Vector3){ 5, 0, 0 }, 2, 2, 2, WHITE);
        
        EndMode3D();
        
        // UI 2D sobre o 3D
        DrawText("Modo 3D - Use o mouse para rotacionar", 10, 10, 20, DARKGRAY);
        DrawFPS(10, 40);
        
        EndDrawing();
    }
    
    CloseWindow();
    return 0;
}
```

## 🎨 Shaders

### Efeitos Visuais com Shaders

```c
#include "raylib.h"

int main(void)
{
    InitWindow(800, 600, "Shaders com Raylib");
    
    // Carregar shader (vertex e fragment)
    Shader shader = LoadShader("resources/shaders/base.vs", 
                               "resources/shaders/grayscale.fs");
    
    // Carregar textura
    Texture2D texture = LoadTexture("resources/player.png");
    
    // Criar render texture para pós-processamento
    RenderTexture2D target = LoadRenderTexture(800, 600);
    
    float time = 0.0f;
    
    SetTargetFPS(60);
    
    while (!WindowShouldClose())
    {
        time += GetFrameTime();
        
        // Atualizar uniform do shader (tempo)
        int timeLoc = GetShaderLocation(shader, "u_time");
        SetShaderValue(shader, timeLoc, &time, SHADER_UNIFORM_FLOAT);
        
        // Desenhar tudo em uma render texture
        BeginTextureMode(target);
        ClearBackground(RAYWHITE);
        
        // Desenhar cena normal
        DrawTexture(texture, 100, 100, WHITE);
        DrawCircle(400, 300, 50, BLUE);
        DrawRectangle(500, 200, 100, 80, GREEN);
        
        EndTextureMode();
        
        // Aplicar shader na tela final
        BeginDrawing();
        ClearBackground(RAYWHITE);
        
        BeginShaderMode(shader);
        DrawTextureRec(target.texture, 
                      (Rectangle){ 0, 0, (float)target.texture.width, (float)-target.texture.height },
                      (Vector2){ 0, 0 }, WHITE);
        EndShaderMode();
        
        DrawText("Shader aplicado na cena", 10, 10, 20, DARKGRAY);
        
        EndDrawing();
    }
    
    // Limpeza
    UnloadShader(shader);
    UnloadTexture(texture);
    UnloadRenderTexture(target);
    
    CloseWindow();
    return 0;
}
```

## 📱 Exemplo Completo: Jogo Pong

### Projeto Final Integrado

```c
#include "raylib.h"
#include <math.h>

typedef struct {
    Vector2 position;
    Vector2 speed;
    float radius;
    bool active;
} Ball;

typedef struct {
    Rectangle rect;
    int score;
    Color color;
} Paddle;

int main(void)
{
    // Inicialização
    const int screenWidth = 800;
    const int screenHeight = 600;
    
    InitWindow(screenWidth, screenHeight, "Pong Game");
    InitAudioDevice();
    
    // Carregar recursos
    Sound hitSound = LoadSound("resources/hit.wav");
    Sound scoreSound = LoadSound("resources/score.wav");
    
    // Inicializar objetos
    Ball ball = {
        .position = { screenWidth/2, screenHeight/2 },
        .speed = { 300, 200 },
        .radius = 8,
        .active = true
    };
    
    Paddle leftPaddle = {
        .rect = { 50, screenHeight/2 - 60, 20, 120 },
        .score = 0,
        .color = BLUE
    };
    
    Paddle rightPaddle = {
        .rect = { screenWidth - 70, screenHeight/2 - 60, 20, 120 },
        .score = 0,
        .color = RED
    };
    
    bool gamePaused = false;
    bool gameStarted = false;
    
    SetTargetFPS(60);
    
    // Game loop
    while (!WindowShouldClose())
    {
        float dt = GetFrameTime();
        
        // Input
        if (IsKeyPressed(KEY_P)) gamePaused = !gamePaused;
        if (IsKeyPressed(KEY_SPACE)) gameStarted = true;
        if (IsKeyPressed(KEY_R)) {
            // Reset game
            ball.position = (Vector2){ screenWidth/2, screenHeight/2 };
            ball.speed = (Vector2){ 300, 200 };
            leftPaddle.score = 0;
            rightPaddle.score = 0;
            gameStarted = true;
        }
        
        if (gameStarted && !gamePaused)
        {
            // Movimento das raquetes
            if (IsKeyDown(KEY_W) && leftPaddle.rect.y > 0)
                leftPaddle.rect.y -= 400 * dt;
            if (IsKeyDown(KEY_S) && leftPaddle.rect.y < screenHeight - leftPaddle.rect.height)
                leftPaddle.rect.y += 400 * dt;
                
            if (IsKeyDown(KEY_UP) && rightPaddle.rect.y > 0)
                rightPaddle.rect.y -= 400 * dt;
            if (IsKeyDown(KEY_DOWN) && rightPaddle.rect.y < screenHeight - rightPaddle.rect.height)
                rightPaddle.rect.y += 400 * dt;
            
            // Movimento da bola
            ball.position.x += ball.speed.x * dt;
            ball.position.y += ball.speed.y * dt;
            
            // Colisão com paredes superior/inferior
            if (ball.position.y - ball.radius <= 0 || 
                ball.position.y + ball.radius >= screenHeight)
            {
                ball.speed.y *= -1;
                PlaySound(hitSound);
            }
            
            // Colisão com raquetes
            if (CheckCollisionCircleRec(ball.position, ball.radius, leftPaddle.rect))
            {
                if (ball.speed.x < 0)
                {
                    ball.speed.x *= -1.1f; // Aumenta velocidade gradualmente
                    // Adicionar efeito baseado onde bateu na raquete
                    float hitPos = (ball.position.y - leftPaddle.rect.y) / leftPaddle.rect.height - 0.5f;
                    ball.speed.y += hitPos * 100;
                    PlaySound(hitSound);
                }
            }
            
            if (CheckCollisionCircleRec(ball.position, ball.radius, rightPaddle.rect))
            {
                if (ball.speed.x > 0)
                {
                    ball.speed.x *= -1.1f;
                    float hitPos = (ball.position.y - rightPaddle.rect.y) / rightPaddle.rect.height - 0.5f;
                    ball.speed.y += hitPos * 100;
                    PlaySound(hitSound);
                }
            }
            
            // Pontuação
            if (ball.position.x - ball.radius <= 0)
            {
                rightPaddle.score++;
                ball.position = (Vector2){ screenWidth/2, screenHeight/2 };
                ball.speed = (Vector2){ -300, 200 };
                PlaySound(scoreSound);
            }
            
            if (ball.position.x + ball.radius >= screenWidth)
            {
                leftPaddle.score++;
                ball.position = (Vector2){ screenWidth/2, screenHeight/2 };
                ball.speed = (Vector2){ 300, 200 };
                PlaySound(scoreSound);
            }
        }
        
        // Desenho
        BeginDrawing();
        ClearBackground(BLACK);
        
        // Linha central
        for (int y = 0; y < screenHeight; y += 20)
        {
            DrawRectangle(screenWidth/2 - 2, y, 4, 10, WHITE);
        }
        
        // Desenhar raquetes
        DrawRectangleRec(leftPaddle.rect, leftPaddle.color);
        DrawRectangleRec(rightPaddle.rect, rightPaddle.color);
        
        // Desenhar bola
        DrawCircleV(ball.position, ball.radius, WHITE);
        
        // Placar
        DrawText(TextFormat("%i", leftPaddle.score), screenWidth/4, 40, 80, BLUE);
        DrawText(TextFormat("%i", rightPaddle.score), 3*screenWidth/4, 40, 80, RED);
        
        // Instruções
        if (!gameStarted)
        {
            DrawText("PONG", screenWidth/2 - 100, 200, 80, WHITE);
            DrawText("Pressione ESPAÇO para começar", 
                    screenWidth/2 - 200, 300, 30, WHITE);
        }
        
        if (gamePaused)
        {
            DrawText("PAUSADO", screenWidth/2 - 120, 250, 60, YELLOW);
        }
        
        DrawText("W/S - Esquerda | Setas - Direita | P - Pausar | R - Reiniciar", 
                10, screenHeight - 30, 20, GRAY);
        
        DrawFPS(10, 10);
        
        EndDrawing();
    }
    
    // Limpeza
    UnloadSound(hitSound);
    UnloadSound(scoreSound);
    CloseAudioDevice();
    CloseWindow();
    
    return 0;
}
```

## 📚 Referência Rápida de Funções

### Core Window
| Função | Descrição |
|--------|-----------|
| `InitWindow()` | Inicializa a janela |
| `WindowShouldClose()` | Verifica se deve fechar |
| `CloseWindow()` | Fecha a janela |
| `SetTargetFPS()` | Define FPS alvo |
| `GetFrameTime()` | Obtém tempo do frame |

### Drawing
| Função | Descrição |
|--------|-----------|
| `BeginDrawing()`/`EndDrawing()` | Inicia/finaliza desenho |
| `ClearBackground()` | Limpa tela com cor |
| `DrawText()` | Desenha texto |
| `DrawFPS()` | Mostra FPS na tela |

### Shapes 2D
| Função | Descrição |
|--------|-----------|
| `DrawPixel()` | Desenha pixel |
| `DrawLine()` | Desenha linha |
| `DrawCircle()` | Desenha círculo |
| `DrawRectangle()` | Desenha retângulo |
| `DrawTriangle()` | Desenha triângulo |
| `DrawPoly()` | Desenha polígono |

### Input
| Função | Descrição |
|--------|-----------|
| `IsKeyPressed()` | Tecla pressionada (uma vez) |
| `IsKeyDown()` | Tecla segura |
| `GetMousePosition()` | Posição do mouse |
| `IsMouseButtonPressed()` | Botão mouse pressionado |

### Textures
| Função | Descrição |
|--------|-----------|
| `LoadTexture()` | Carrega textura |
| `UnloadTexture()` | Descarrega textura |
| `DrawTexture()` | Desenha textura |
| `DrawTextureEx()` | Desenha com transformações |

### Audio
| Função | Descrição |
|--------|-----------|
| `InitAudioDevice()` | Inicia áudio |
| `LoadSound()` | Carrega som |
| `PlaySound()` | Toca som |
| `LoadMusicStream()` | Carrega música |
| `UpdateMusicStream()` | Atualiza música |

### Collision
| Função | Descrição |
|--------|-----------|
| `CheckCollisionRecs()` | Colisão retângulo-retângulo |
| `CheckCollisionCircles()` | Colisão círculo-círculo |
| `CheckCollisionCircleRec()` | Colisão círculo-retângulo |

## 🎯 Dicas e Boas Práticas

1. **Organização**: Separe seu código em arquivos .c e .h
2. **Recursos**: Sempre verifique se arquivos existem antes de carregar
3. **Memória**: Sempre descarregue recursos ao final
4. **Performance**: Use `SetTargetFPS()` para controlar performance
5. **Debug**: Use `DrawFPS()` e `TraceLog()` para debug

```c
// Exemplo de log
TraceLog(LOG_INFO, "Jogo iniciado com sucesso!");
TraceLog(LOG_WARNING, "Recurso não encontrado: %s", filename);
TraceLog(LOG_ERROR, "Falha ao carregar textura!");
```

## 📦 Estrutura de Projeto Recomendada

```
meu_jogo/
├── src/
│   ├── main.c
│   ├── player.c
│   ├── player.h
│   ├── enemy.c
│   └── enemy.h
├── resources/
│   ├── images/
│   ├── sounds/
│   ├── fonts/
│   └── shaders/
├── Makefile
└── README.md
```

## 🚀 Conclusão

Raylib é uma biblioteca incrivelmente versátil e fácil de usar. Com este manual, você tem todas as ferramentas necessárias para criar desde jogos simples até aplicações multimídia complexas.

### Próximos Passos:
- 📖 Leia a documentação oficial: [https://www.raylib.com/](https://www.raylib.com/)
- 🎮 Explore os exemplos na pasta `examples/`
- 👥 Participe da comunidade no Discord
- 🛠️ Experimente criar seu próprio jogo!

**Lembre-se**: A melhor forma de aprender é praticando. Comece com projetos pequenos e vá aumentando a complexidade gradualmente. Divirta-se programando com Raylib! 🎮✨
