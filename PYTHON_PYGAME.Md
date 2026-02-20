# **Manual Completo de Pygame**

## **Índice**
1. [Introdução](#introdução)
2. [Instalação](#instalação)
3. [Primeiros Passos](#primeiros-passos)
4. [Estrutura Básica de um Jogo](#estrutura-básica-de-um-jogo)
5. [Desenhando na Tela](#desenhando-na-tela)
6. [Trabalhando com Imagens](#trabalhando-com-imagens)
7. [Eventos e Controles](#eventos-e-controles)
8. [Som e Música](#som-e-música)
9. [Sprites e Grupos](#sprites-e-grupos)
10. [Colisões](#colisões)
11. [Fontes e Texto](#fontes-e-texto)
12. [Tópicos Avançados](#tópicos-avançados)
13. [Projetos Práticos](#projetos-práticos)
14. [Referências](#referências)

---

## **Introdução**

Pygame é uma biblioteca multiplataforma para desenvolvimento de jogos em Python. Construída sobre a SDL (Simple DirectMedia Layer), ela oferece funcionalidades para:

- Gráficos 2D
- Reprodução de áudio
- Manipulação de eventos (teclado, mouse, joystick)
- Detecção de colisões
- E muito mais!

**Por que usar Pygame?**
- ✅ Fácil de aprender para iniciantes
- ✅ Código aberto e gratuito
- ✅ Ótima documentação e comunidade ativa
- ✅ Ideal para jogos 2D e prototipagem rápida

---

## **Instalação**

### **Windows/Mac/Linux**
```bash
pip install pygame
```

### **Verificando a instalação**
```python
import pygame
print(pygame.version.ver)  # Mostra a versão instalada
```

### **Teste rápido**
```python
import pygame
pygame.init()
print("Pygame instalado com sucesso!")
```

---

## **Primeiros Passos**

### **Seu primeiro programa Pygame**
```python
import pygame
import sys

# Inicialização
pygame.init()

# Configurações da tela
LARGURA, ALTURA = 800, 600
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Meu Primeiro Jogo")

# Cores
BRANCO = (255, 255, 255)

# Loop principal
while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    # Preenche a tela com branco
    tela.fill(BRANCO)
    
    # Atualiza a tela
    pygame.display.flip()
```

---

## **Estrutura Básica de um Jogo**

```python
import pygame
import sys

# Inicialização
pygame.init()

# Constantes
LARGURA, ALTURA = 800, 600
FPS = 60  # Frames por segundo

# Configurações da tela
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Estrutura Básica")
relogio = pygame.time.Clock()

# Loop principal
while True:
    # Controla a velocidade do jogo
    relogio.tick(FPS)
    
    # Processa eventos
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    # Atualiza lógica do jogo
    # (movimentação, colisões, etc.)
    
    # Desenha na tela
    tela.fill((0, 0, 0))  # Preto
    # (desenhar objetos)
    
    # Atualiza a tela
    pygame.display.flip()
```

---

## **Desenhando na Tela**

### **Cores em RGB**
```python
# Formato: (RED, GREEN, BLUE)
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
VERDE = (0, 255, 0)
AZUL = (0, 0, 255)
AMARELO = (255, 255, 0)
CIANO = (0, 255, 255)
MAGENTA = (255, 0, 255)
```

### **Formas Geométricas**
```python
# Retângulo
pygame.draw.rect(tela, VERMELHO, (50, 50, 100, 100))  # (x, y, largura, altura)
pygame.draw.rect(tela, AZUL, pygame.Rect(200, 50, 100, 100))  # Usando Rect

# Círculo
pygame.draw.circle(tela, VERDE, (400, 300), 50)  # (centro_x, centro_y), raio

# Linha
pygame.draw.line(tela, BRANCO, (0, 0), (800, 600), 5)  # inicio, fim, espessura

# Polígono
pontos = [(100, 100), (200, 150), (150, 250)]
pygame.draw.polygon(tela, AMARELO, pontos)

# Elipse
pygame.draw.ellipse(tela, CIANO, (300, 300, 150, 100))

# Arco
pygame.draw.arc(tela, MAGENTA, (400, 200, 200, 150), 0, 3.14, 5)
```

### **Preenchimento vs Contorno**
```python
# Formas preenchidas (padrão)
pygame.draw.rect(tela, VERMELHO, (50, 50, 100, 100))

# Apenas contorno (adicionar espessura)
pygame.draw.rect(tela, VERMELHO, (50, 50, 100, 100), 3)  # Contorno de 3px
pygame.draw.circle(tela, AZUL, (200, 200), 50, 2)  # Contorno de 2px
```

---

## **Trabalhando com Imagens**

### **Carregando e exibindo imagens**
```python
# Carrega a imagem
imagem = pygame.image.load("imagem.png")

# Redimensiona
imagem = pygame.transform.scale(imagem, (100, 100))

# Rotaciona
imagem = pygame.transform.rotate(imagem, 45)

# Obtém retângulo para posicionamento
rect_imagem = imagem.get_rect()
rect_imagem.center = (LARGURA // 2, ALTURA // 2)

# Desenha na tela
tela.blit(imagem, rect_imagem)
```

### **Manipulação de imagens**
```python
# Transparência (se a imagem não tiver canal alpha)
imagem.set_alpha(128)  # 0-255 (transparente-opaco)

# Espelhar horizontal/vertical
imagem_h = pygame.transform.flip(imagem, True, False)  # Horizontal
imagem_v = pygame.transform.flip(imagem, False, True)  # Vertical

# Escala suave (melhor qualidade)
imagem = pygame.transform.smoothscale(imagem, (200, 200))
```

---

## **Eventos e Controles**

### **Eventos do teclado**
```python
for evento in pygame.event.get():
    if evento.type == pygame.KEYDOWN:
        if evento.key == pygame.K_SPACE:
            print("Espaço pressionado!")
        elif evento.key == pygame.K_LEFT:
            print("Seta esquerda")
        elif evento.key == pygame.K_RIGHT:
            print("Seta direita")
        elif evento.key == pygame.K_ESCAPE:
            pygame.quit()
            sys.exit()
    
    if evento.type == pygame.KEYUP:
        print("Tecla solta")
```

### **Teclas mantidas pressionadas**
```python
# Melhor para movimento contínuo
teclas = pygame.key.get_pressed()

if teclas[pygame.K_LEFT]:
    x -= 5
if teclas[pygame.K_RIGHT]:
    x += 5
if teclas[pygame.K_UP]:
    y -= 5
if teclas[pygame.K_DOWN]:
    y += 5
```

### **Eventos do mouse**
```python
for evento in pygame.event.get():
    if evento.type == pygame.MOUSEBUTTONDOWN:
        if evento.button == 1:  # Botão esquerdo
            print(f"Clique em: {evento.pos}")
        elif evento.button == 3:  # Botão direito
            print("Botão direito")
    
    if evento.type == pygame.MOUSEBUTTONUP:
        print("Botão solto")
    
    if evento.type == pygame.MOUSEMOTION:
        print(f"Mouse em: {evento.pos}")
```

### **Estado do mouse**
```python
# Posição atual
x, y = pygame.mouse.get_pos()

# Botões pressionados
botoes = pygame.mouse.get_pressed()
if botoes[0]:  # Botão esquerdo
    print("Botão esquerdo pressionado")
```

---

## **Som e Música**

### **Efeitos sonoros (wav)**
```python
# Carrega som
som_pulo = pygame.mixer.Sound("pulo.wav")
som_colisao = pygame.mixer.Sound("colisao.wav")

# Toca som
som_pulo.play()

# Ajusta volume (0.0 a 1.0)
som_pulo.set_volume(0.5)
```

### **Música de fundo (mp3/ogg)**
```python
# Carrega música
pygame.mixer.music.load("musica_fundo.mp3")

# Configura volume
pygame.mixer.music.set_volume(0.3)

# Toca música (-1 para repetir infinitamente)
pygame.mixer.music.play(-1)

# Controles
pygame.mixer.music.pause()   # Pausa
pygame.mixer.music.unpause() # Continua
pygame.mixer.music.stop()    # Para
```

---

## **Sprites e Grupos**

### **Criando uma classe Sprite**
```python
class Jogador(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill((0, 255, 0))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.velocidade = 5
    
    def update(self):
        teclas = pygame.key.get_pressed()
        if teclas[pygame.K_LEFT]:
            self.rect.x -= self.velocidade
        if teclas[pygame.K_RIGHT]:
            self.rect.x += self.velocidade
        if teclas[pygame.K_UP]:
            self.rect.y -= self.velocidade
        if teclas[pygame.K_DOWN]:
            self.rect.y += self.velocidade
        
        # Mantém dentro da tela
        self.rect.clamp_ip(tela.get_rect())
```

### **Grupos de sprites**
```python
# Cria grupos
todos_sprites = pygame.sprite.Group()
inimigos = pygame.sprite.Group()
projeteis = pygame.sprite.Group()

# Adiciona sprites
jogador = Jogador(400, 300)
todos_sprites.add(jogador)

for i in range(5):
    inimigo = Inimigo(i * 100, 100)
    todos_sprites.add(inimigo)
    inimigos.add(inimigo)

# Atualiza todos
todos_sprites.update()

# Desenha todos
todos_sprites.draw(tela)
```

---

## **Colisões**

### **Detecção básica de colisão**
```python
# Colisão entre dois retângulos
if jogador.rect.colliderect(inimigo.rect):
    print("Colidiu!")

# Colisão com ponto
if jogador.rect.collidepoint(mouse_x, mouse_y):
    print("Mouse sobre o jogador")
```

### **Colisões com grupos**
```python
# Verifica colisão entre jogador e grupo de inimigos
colisoes = pygame.sprite.spritecollide(jogador, inimigos, True)
for inimigo in colisoes:
    print("Jogador colidiu com inimigo!")
    jogador.vida -= 10

# Colisão entre dois grupos
colisoes = pygame.sprite.groupcollide(projeteis, inimigos, True, True)
for projetil, inimigos_colididos in colisoes.items():
    print(f"Projétil acertou {len(inimigos_colididos)} inimigos!")
```

### **Colisão precisa (máscara)**
```python
# Criando máscara a partir da imagem
jogador.mask = pygame.mask.from_surface(jogador.image)
inimigo.mask = pygame.mask.from_surface(inimigo.image)

# Colisão baseada em máscara (mais precisa)
offset = (inimigo.rect.x - jogador.rect.x, inimigo.rect.y - jogador.rect.y)
if jogador.mask.overlap(inimigo.mask, offset):
    print("Colisão precisa!")
```

---

## **Fontes e Texto**

### **Fontes padrão do sistema**
```python
# Fonte padrão
fonte = pygame.font.Font(None, 36)  # None = fonte padrão, 36 = tamanho

# Criando texto
texto = fonte.render("Olá Mundo!", True, BRANCO)  # Texto, anti-aliasing, cor

# Posicionando
rect_texto = texto.get_rect()
rect_texto.center = (400, 300)

# Desenhando
tela.blit(texto, rect_texto)
```

### **Fontes personalizadas**
```python
# Carrega fonte específica
fonte = pygame.font.Font("arial.ttf", 48)

# Texto com fundo
texto = fonte.render("Pygame", True, AMARELO, AZUL)  # Texto, anti-aliasing, cor, fundo
```

### **Relógio e FPS**
```python
# Mostrar FPS na tela
fonte = pygame.font.Font(None, 24)
fps_texto = fonte.render(f"FPS: {int(relogio.get_fps())}", True, BRANCO)
tela.blit(fps_texto, (10, 10))
```

---

## **Tópicos Avançados**

### **Animações simples**
```python
class Animacao:
    def __init__(self, frames, intervalo=100):
        self.frames = frames  # Lista de imagens
        self.intervalo = intervalo  # ms entre frames
        self.frame_atual = 0
        self.ultimo_tempo = pygame.time.get_ticks()
    
    def update(self):
        agora = pygame.time.get_ticks()
        if agora - self.ultimo_tempo > self.intervalo:
            self.frame_atual = (self.frame_atual + 1) % len(self.frames)
            self.ultimo_tempo = agora
    
    def get_frame(self):
        return self.frames[self.frame_atual]
```

### **Câmera/Scroll**
```python
class Camera:
    def __init__(self, largura, altura):
        self.camera = pygame.Rect(0, 0, largura, altura)
        self.largura = largura
        self.altura = altura
    
    def aplicar(self, entidade):
        return entidade.rect.move(self.camera.topleft)
    
    def update(self, alvo):
        x = -alvo.rect.centerx + self.largura // 2
        y = -alvo.rect.centery + self.altura // 2
        
        # Limita a câmera
        x = min(0, x)  # Esquerda
        x = max(-(self.largura_mundo - self.largura), x)  # Direita
        y = min(0, y)  # Topo
        y = max(-(self.altura_mundo - self.altura), y)  # Base
        
        self.camera = pygame.Rect(x, y, self.largura, self.altura)
```

### **Tiled (mapas)**
```python
import pytmx

# Carrega mapa criado no Tiled
tmx_data = pytmx.load_pygame("mapa.tmx")
layer = tmx_data.get_layer_by_name("Cenario")

# Desenha tiles
for x, y, image in layer.tiles():
    tela.blit(image, (x * tmx_data.tilewidth, y * tmx_data.tileheight))
```

---

## **Projetos Práticos**

### **Projeto 1: Pong**

```python
import pygame
import sys

# Inicialização
pygame.init()

# Constantes
LARGURA, ALTURA = 800, 600
BRANCO = (255, 255, 255)
PRETO = (0, 0, 0)
FPS = 60

tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Pong")
relogio = pygame.time.Clock()

# Palhetas
palheta_esquerda = pygame.Rect(50, ALTURA//2 - 60, 20, 120)
palheta_direita = pygame.Rect(LARGURA - 70, ALTURA//2 - 60, 20, 120)

# Bola
bola = pygame.Rect(LARGURA//2 - 15, ALTURA//2 - 15, 30, 30)
vel_bola_x, vel_bola_y = 7, 7

# Placar
fonte = pygame.font.Font(None, 74)
pontos_esq = pontos_dir = 0

while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    # Movimento das palhetas
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_w] and palheta_esquerda.top > 0:
        palheta_esquerda.y -= 5
    if teclas[pygame.K_s] and palheta_esquerda.bottom < ALTURA:
        palheta_esquerda.y += 5
    if teclas[pygame.K_UP] and palheta_direita.top > 0:
        palheta_direita.y -= 5
    if teclas[pygame.K_DOWN] and palheta_direita.bottom < ALTURA:
        palheta_direita.y += 5
    
    # Movimento da bola
    bola.x += vel_bola_x
    bola.y += vel_bola_y
    
    # Colisão com teto/chão
    if bola.top <= 0 or bola.bottom >= ALTURA:
        vel_bola_y *= -1
    
    # Colisão com palhetas
    if bola.colliderect(palheta_esquerda) or bola.colliderect(palheta_direita):
        vel_bola_x *= -1
    
    # Pontuação
    if bola.left <= 0:
        pontos_dir += 1
        bola.center = (LARGURA//2, ALTURA//2)
        vel_bola_x *= -1
    if bola.right >= LARGURA:
        pontos_esq += 1
        bola.center = (LARGURA//2, ALTURA//2)
        vel_bola_x *= -1
    
    # Desenho
    tela.fill(PRETO)
    pygame.draw.rect(tela, BRANCO, palheta_esquerda)
    pygame.draw.rect(tela, BRANCO, palheta_direita)
    pygame.draw.ellipse(tela, BRANCO, bola)
    pygame.draw.aaline(tela, BRANCO, (LARGURA//2, 0), (LARGURA//2, ALTURA))
    
    # Placar
    texto_esq = fonte.render(str(pontos_esq), True, BRANCO)
    texto_dir = fonte.render(str(pontos_dir), True, BRANCO)
    tela.blit(texto_esq, (LARGURA//4, 50))
    tela.blit(texto_dir, (3*LARGURA//4, 50))
    
    pygame.display.flip()
    relogio.tick(FPS)
```

### **Projeto 2: Jogo da Cobrinha**

```python
import pygame
import sys
import random

pygame.init()

LARGURA, ALTURA = 600, 600
TAMANHO_CELULA = 20
LARGURA_GRADE = LARGURA // TAMANHO_CELULA
ALTURA_GRADE = ALTURA // TAMANHO_CELULA

PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERDE = (0, 255, 0)
VERMELHO = (255, 0, 0)

tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Jogo da Cobrinha")
relogio = pygame.time.Clock()

def desenhar_grade():
    for x in range(0, LARGURA, TAMANHO_CELULA):
        pygame.draw.line(tela, (40, 40, 40), (x, 0), (x, ALTURA))
    for y in range(0, ALTURA, TAMANHO_CELULA):
        pygame.draw.line(tela, (40, 40, 40), (0, y), (LARGURA, y))

class Cobra:
    def __init__(self):
        self.corpo = [(5, 5), (4, 5), (3, 5)]
        self.direcao = (1, 0)
    
    def mover(self):
        cabeca = self.corpo[0]
        nova_cabeca = (cabeca[0] + self.direcao[0], cabeca[1] + self.direcao[1])
        self.corpo.insert(0, nova_cabeca)
        self.corpo.pop()
    
    def crescer(self):
        self.corpo.append(self.corpo[-1])
    
    def desenhar(self):
        for segmento in self.corpo:
            rect = pygame.Rect(segmento[0]*TAMANHO_CELULA, segmento[1]*TAMANHO_CELULA, 
                              TAMANHO_CELULA-2, TAMANHO_CELULA-2)
            pygame.draw.rect(tela, VERDE, rect)
            pygame.draw.rect(tela, BRANCO, rect, 1)
    
    def colisao_propria(self):
        return self.corpo[0] in self.corpo[1:]

def gerar_comida(cobra):
    while True:
        pos = (random.randint(0, LARGURA_GRADE-1), random.randint(0, ALTURA_GRADE-1))
        if pos not in cobra.corpo:
            return pos

cobra = Cobra()
comida = gerar_comida(cobra)
fonte = pygame.font.Font(None, 36)

while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if evento.type == pygame.KEYDOWN:
            if evento.key == pygame.K_UP and cobra.direcao != (0, 1):
                cobra.direcao = (0, -1)
            elif evento.key == pygame.K_DOWN and cobra.direcao != (0, -1):
                cobra.direcao = (0, 1)
            elif evento.key == pygame.K_LEFT and cobra.direcao != (1, 0):
                cobra.direcao = (-1, 0)
            elif evento.key == pygame.K_RIGHT and cobra.direcao != (-1, 0):
                cobra.direcao = (1, 0)
    
    cobra.mover()
    
    # Verifica colisão com comida
    if cobra.corpo[0] == comida:
        cobra.crescer()
        comida = gerar_comida(cobra)
    
    # Verifica colisão com paredes
    cabeca = cobra.corpo[0]
    if cabeca[0] < 0 or cabeca[0] >= LARGURA_GRADE or cabeca[1] < 0 or cabeca[1] >= ALTURA_GRADE:
        break
    
    # Verifica colisão com próprio corpo
    if cobra.colisao_propria():
        break
    
    tela.fill(PRETO)
    desenhar_grade()
    
    # Desenha comida
    rect_comida = pygame.Rect(comida[0]*TAMANHO_CELULA, comida[1]*TAMANHO_CELULA, 
                              TAMANHO_CELULA-2, TAMANHO_CELULA-2)
    pygame.draw.rect(tela, VERMELHO, rect_comida)
    
    cobra.desenhar()
    
    # Mostra pontuação
    texto = fonte.render(f"Pontos: {len(cobra.corpo)-3}", True, BRANCO)
    tela.blit(texto, (10, 10))
    
    pygame.display.flip()
    relogio.tick(10)

# Game Over
tela.fill(PRETO)
texto_gameover = fonte.render("GAME OVER", True, VERMELHO)
texto_pontos = fonte.render(f"Pontuação final: {len(cobra.corpo)-3}", True, BRANCO)
tela.blit(texto_gameover, (LARGURA//2 - 100, ALTURA//2 - 50))
tela.blit(texto_pontos, (LARGURA//2 - 120, ALTURA//2))
pygame.display.flip()
pygame.time.wait(3000)
pygame.quit()
sys.exit()
```

---

## **Referências**

### **Documentação Oficial**
- [Pygame Documentation](https://www.pygame.org/docs/)
- [Pygame GitHub](https://github.com/pygame/pygame)

### **Recursos Úteis**
- **Imagens**: [OpenGameArt](https://opengameart.org/), [Itch.io](https://itch.io/game-assets)
- **Sons**: [Freesound](https://freesound.org/), [Zapsplat](https://www.zapsplat.com/)
- **Fontes**: [Google Fonts](https://fonts.google.com/)
- **Tutoriais**: [Real Python](https://realpython.com/tutorials/gamedev/), [KidsCanCode](https://kidscancode.org/)

### **Dicas Finais**
1. Sempre use `pygame.init()` no início
2. Mantenha o FPS constante com `pygame.time.Clock()`
3. Organize seu código em classes e funções
4. Teste frequentemente durante o desenvolvimento
5. Otimize desenhos e colisões para jogos maiores
6. Use sprites e grupos para jogos complexos
7. Sempre trate o evento `QUIT` para fechar corretamente

---

**Bons jogos! 🎮**
