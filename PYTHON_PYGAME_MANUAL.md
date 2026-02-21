
## Manual Completo da Biblioteca Pygame

### 1. Introdução ao Pygame

Pygame é um conjunto de módulos Python projetados para criar jogos e aplicações multimídia. Ele utiliza a biblioteca SDL (Simple DirectMedia Layer) para fornecer acesso cross-platform aos componentes de hardware do sistema, como vídeo, som, mouse, teclado e joystick .

#### Principais Características
- **Gratuito e Open Source**: Distribuído sob licença LGPL 
- **Multiplataforma**: Funciona em Windows, Linux, Mac e outros sistemas 
- **Leve e Eficiente**: Ideal para jogos 2D 
- **Controle Total**: Diferente de outras bibliotecas, você mantém controle completo sobre o programa 

### 2. Estrutura Básica de um Programa Pygame

Todo programa Pygame segue uma estrutura fundamental. Abaixo está o template básico que serve como ponto de partida para qualquer projeto :

```python
import pygame
import sys
from pygame.locals import *

# Inicialização
pygame.init()

# Configurações da Janela
WINDOW_WIDTH = 800
WINDOW_HEIGHT = 600
FPS = 60

# Cores (formato RGB)
BACKGROUND = (255, 255, 255)  # Branco

# Configuração da tela
screen = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
pygame.display.set_caption("Meu Jogo")

# Clock para controle de FPS
clock = pygame.time.Clock()

# Loop Principal do Jogo
running = True
while running:
    # 1. Tratamento de Eventos
    for event in pygame.event.get():
        if event.type == QUIT:
            running = False
    
    # 2. Lógica do Jogo (atualização de estado)
    
    # 3. Renderização
    screen.fill(BACKGROUND)
    # Código de desenho aqui...
    
    pygame.display.update()
    clock.tick(FPS)

# Encerramento
pygame.quit()
sys.exit()
```

### 3. Módulos Principais e Suas Funções

Aqui está uma lista completa dos principais módulos do Pygame com suas funções mais importantes :

#### pygame (Módulo Principal)

| Função | Descrição |
|--------|-----------|
| `pygame.init()` | Inicializa todos os módulos pygame importados  |
| `pygame.quit()` | Desinicializa todos os módulos  |
| `pygame.get_init()` | Verifica se o pygame está inicializado  |
| `pygame.error()` | Exceção padrão do pygame  |
| `pygame.get_error()` | Obtém a mensagem de erro atual  |
| `pygame.set_error(msg)` | Define a mensagem de erro atual  |

#### pygame.display (Gerenciamento da Janela)

| Função | Descrição |
|--------|-----------|
| `pygame.display.set_mode((w, h))` | Cria a janela do jogo  |
| `pygame.display.set_caption(título)` | Define o título da janela  |
| `pygame.display.flip()` | Atualiza toda a tela  |
| `pygame.display.update(rect)` | Atualiza parte da tela  |
| `pygame.display.get_surface()` | Retorna a superfície atual da tela |
| `pygame.display.set_icon(superfície)` | Define o ícone da janela |
| `pygame.display.get_driver()` | Retorna o nome do driver de vídeo |

#### pygame.Surface (Superfícies de Desenho)

A classe mais importante do pygame. Tudo que é visual é uma Surface .

| Método | Descrição |
|--------|-----------|
| `pygame.Surface((w, h))` | Cria uma nova superfície  |
| `surface.blit(origem, posição)` | Desenha uma superfície em outra  |
| `surface.fill(cor)` | Preenche a superfície com uma cor  |
| `surface.set_at((x,y), cor)` | Define a cor de um pixel |
| `surface.get_at((x,y))` | Obtém a cor de um pixel |
| `surface.get_rect()` | Obtém o retângulo da superfície  |
| `surface.convert()` | Converte para formato ideal para velocidade  |
| `surface.convert_alpha()` | Converte mantendo canal alpha |
| `surface.get_size()` | Retorna (largura, altura) |
| `surface.get_width()` | Retorna a largura |
| `surface.get_height()` | Retorna a altura |
| `surface.subsurface(rect)` | Cria uma sub-superfície |

#### pygame.Rect (Retângulos)

Utilitário essencial para posicionamento e detecção de colisões .

**Atributos Virtuais do Rect** :

| Atributo | Descrição |
|----------|-----------|
| `rect.x, rect.y` | Coordenadas do canto superior esquerdo |
| `rect.left, rect.right` | Posições das bordas esquerda e direita |
| `rect.top, rect.bottom` | Posições das bordas superior e inferior |
| `rect.centerx, rect.centery` | Coordenadas do centro |
| `rect.width, rect.height` | Dimensões do retângulo |
| `rect.size` | Tupla (largura, altura) |
| `rect.topleft` | Tupla (left, top) |
| `rect.topright` | Tupla (right, top) |
| `rect.bottomleft` | Tupla (left, bottom) |
| `rect.bottomright` | Tupla (right, bottom) |
| `rect.midtop` | Tupla (centerx, top) |
| `rect.midbottom` | Tupla (centerx, bottom) |
| `rect.midleft` | Tupla (left, centery) |
| `rect.midright` | Tupla (right, centery) |

**Métodos de Rect**:

| Método | Descrição |
|--------|-----------|
| `rect.move(dx, dy)` | Move o retângulo (retorna novo) |
| `rect.move_ip(dx, dy)` | Move o retângulo in-place |
| `rect.collidepoint(x, y)` | Verifica se ponto está dentro |
| `rect.colliderect(outro_rect)` | Verifica colisão com outro rect |
| `rect.collidelist(lista_rects)` | Retorna índice do primeiro rect colidido |
| `rect.inflate(x, y)` | Aumenta/diminui o retângulo |
| `rect.clamp(limite)` | Move para dentro do limite |

#### pygame.draw (Desenho de Formas)

| Função | Descrição |
|--------|-----------|
| `pygame.draw.rect(superfície, cor, rect)` | Desenha retângulo |
| `pygame.draw.circle(superfície, cor, centro, raio)` | Desenha círculo  |
| `pygame.draw.line(superfície, cor, inicio, fim)` | Desenha linha |
| `pygame.draw.lines(superfície, cor, fechado, pontos)` | Desenha múltiplas linhas |
| `pygame.draw.polygon(superfície, cor, pontos)` | Desenha polígono |
| `pygame.draw.ellipse(superfície, cor, rect)` | Desenha elipse |
| `pygame.draw.arc(superfície, cor, rect, inicio, fim)` | Desenha arco |

#### pygame.event (Gerenciamento de Eventos)

| Função | Descrição |
|--------|-----------|
| `pygame.event.get()` | Retorna lista de eventos  |
| `pygame.event.poll()` | Retorna um evento da fila |
| `pygame.event.wait()` | Aguarda um evento |
| `pygame.event.post(evento)` | Adiciona evento à fila |
| `pygame.event.clear()` | Limpa fila de eventos |
| `pygame.event.set_blocked(tipo)` | Bloqueia tipo de evento |
| `pygame.event.set_allowed(tipo)` | Permite tipo de evento |

**Tipos de Eventos Comuns** :

| Constante | Descrição |
|-----------|-----------|
| `QUIT` | Janela fechada pelo usuário |
| `KEYDOWN` | Tecla pressionada |
| `KEYUP` | Tecla solta |
| `MOUSEMOTION` | Mouse movido |
| `MOUSEBUTTONDOWN` | Botão do mouse pressionado |
| `MOUSEBUTTONUP` | Botão do mouse solto |
| `JOYAXISMOTION` | Movimento do eixo do joystick |

#### pygame.key (Teclado)

| Função | Descrição |
|--------|-----------|
| `pygame.key.get_pressed()` | Retorna estado de todas as teclas  |
| `pygame.key.get_mods()` | Retorna teclas modificadoras pressionadas |
| `pygame.key.set_repeat()` | Define repetição de teclas |
| `pygame.key.name(tecla)` | Retorna nome da tecla |

**Constantes de Teclas Comuns** :

| Constante | Tecla |
|-----------|-------|
| `K_w, K_a, K_s, K_d` | Teclas WASD |
| `K_UP, K_DOWN, K_LEFT, K_RIGHT` | Setas direcionais |
| `K_SPACE` | Espaço |
| `K_RETURN` | Enter |
| `K_ESCAPE` | ESC |
| `K_LSHIFT, K_RSHIFT` | Shift |
| `K_LCTRL, K_RCTRL` | Ctrl |

#### pygame.mouse (Mouse)

| Função | Descrição |
|--------|-----------|
| `pygame.mouse.get_pos()` | Retorna posição (x, y) do mouse |
| `pygame.mouse.get_pressed()` | Retorna estado dos botões |
| `pygame.mouse.set_pos(x, y)` | Define posição do mouse |
| `pygame.mouse.set_visible(bool)` | Mostra/esconde cursor |
| `pygame.mouse.get_visible()` | Verifica se cursor está visível |

#### pygame.time (Controle de Tempo)

| Função | Descrição |
|--------|-----------|
| `pygame.time.Clock()` | Cria objeto Clock  |
| `clock.tick(FPS)` | Controla frames por segundo  |
| `clock.get_fps()` | Retorna FPS atual |
| `pygame.time.get_ticks()` | Retorna milissegundos desde init() |
| `pygame.time.delay()` | Pausa por milissegundos |
| `pygame.time.wait()` | Pausa por milissegundos (menos preciso) |

#### pygame.image (Carregamento de Imagens)

| Função | Descrição |
|--------|-----------|
| `pygame.image.load(arquivo)` | Carrega imagem  |
| `pygame.image.save(superfície, arquivo)` | Salva imagem |

**Formatos suportados**: PNG, JPG, GIF, BMP, TGA, PCX, TIF, LBM, PBM, XPM 

#### pygame.font (Textos)

| Função | Descrição |
|--------|-----------|
| `pygame.font.init()` | Inicializa módulo de fontes |
| `pygame.font.get_init()` | Verifica inicialização |
| `pygame.font.get_default_font()` | Retorna fonte padrão |
| `pygame.font.get_fonts()` | Lista fontes disponíveis |
| `pygame.font.Font(nome, tamanho)` | Carrega fonte TrueType  |
| `font.render(texto, anti-alias, cor)` | Cria superfície com texto  |
| `font.size(texto)` | Retorna dimensões do texto |

#### pygame.mixer (Áudio)

| Função | Descrição |
|--------|-----------|
| `pygame.mixer.init()` | Inicializa mixer |
| `pygame.mixer.quit()` | Finaliza mixer |
| `pygame.mixer.Sound(arquivo)` | Carrega efeito sonoro |
| `sound.play()` | Toca som |
| `sound.stop()` | Para som |
| `sound.set_volume(volume)` | Define volume (0.0 a 1.0) |
| `pygame.mixer.music.load(arquivo)` | Carrega música de fundo |
| `pygame.mixer.music.play()` | Toca música |
| `pygame.mixer.music.stop()` | Para música |
| `pygame.mixer.music.pause()` | Pausa música |
| `pygame.mixer.music.unpause()` | Continua música |
| `pygame.mixer.music.set_volume(volume)` | Define volume da música |

#### pygame.transform (Transformações)

| Função | Descrição |
|--------|-----------|
| `pygame.transform.scale(superfície, tamanho)` | Redimensiona  |
| `pygame.transform.rotate(superfície, ângulo)` | Rotaciona  |
| `pygame.transform.flip(superfície, x_bool, y_bool)` | Inverte imagem |
| `pygame.transform.rotozoom(superfície, ângulo, escala)` | Rotaciona e escala |
| `pygame.transform.smoothscale()` | Redimensiona com suavização |

#### pygame.sprite (Sprites)

Sistema para gerenciar objetos do jogo .

| Classe/Método | Descrição |
|---------------|-----------|
| `pygame.sprite.Sprite()` | Classe base para sprites |
| `sprite.update()` | Método para atualizar sprite |
| `pygame.sprite.Group()` | Grupo de sprites |
| `group.add(sprites)` | Adiciona sprites ao grupo |
| `group.remove(sprites)` | Remove sprites |
| `group.draw(superfície)` | Desenha todos sprites |
| `group.update()` | Atualiza todos sprites |
| `pygame.sprite.spritecollide(sprite, grupo, dokill)` | Detecção de colisão |

#### pygame.Color (Cores)

| Método | Descrição |
|--------|-----------|
| `pygame.Color(r, g, b)` | Cria cor RGB  |
| `pygame.Color(r, g, b, a)` | Cria cor RGBA (com transparência) |
| `color.r, color.g, color.b, color.a` | Componentes da cor |
| `color.cmy` | Representação CMY |
| `color.hsva` | Representação HSV |
| `color.hsla` | Representação HSL |
| `color.normalize()` | Normaliza para valores 0-1 |

### 4. Sistema de Coordenadas

O Pygame utiliza um sistema de coordenadas onde o ponto (0, 0) está no **canto superior esquerdo** da tela :

- **X aumenta**: movendo para a direita
- **Y aumenta**: movendo para baixo
- **Canto inferior direito**: (largura, altura)

### 5. Cores no Pygame

As cores são definidas como tuplas RGB (Red, Green, Blue) com valores de 0 a 255 :

```python
# Cores básicas
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)
VERDE = (0, 255, 0)
AZUL = (0, 0, 255)
AMARELO = (255, 255, 0)
CIANO = (0, 255, 255)
MAGENTA = (255, 0, 255)
CINZA = (128, 128, 128)
```

Para cores com transparência (alpha), use uma tupla de 4 valores (RGBA):
```python
VERMELHO_TRANSPARENTE = (255, 0, 0, 128)  # 50% transparente
```

### 6. Exemplo Completo: Bola Quicando

Aqui está um exemplo completo que demonstra vários conceitos fundamentais :

```python
import pygame
import sys
from pygame.locals import *

# Inicialização
pygame.init()

# Configurações
LARGURA, ALTURA = 800, 600
screen = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Bola Quicando")
clock = pygame.time.Clock()

# Cores
BRANCO = (255, 255, 255)
VERMELHO = (255, 0, 0)

# Bola
posicao = pygame.Vector2(LARGURA // 2, ALTURA // 2)
velocidade = pygame.Vector2(5, 7)
raio = 20

# Loop principal
while True:
    # Eventos
    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
    
    # Atualização
    posicao += velocidade
    
    # Colisão com bordas
    if posicao.x - raio <= 0 or posicao.x + raio >= LARGURA:
        velocidade.x *= -1
    if posicao.y - raio <= 0 or posicao.y + raio >= ALTURA:
        velocidade.y *= -1
    
    # Renderização
    screen.fill(BRANCO)
    pygame.draw.circle(screen, VERMELHO, (int(posicao.x), int(posicao.y)), raio)
    
    pygame.display.update()
    clock.tick(60)
```

### 7. Dicas Importantes de Performance

1. **Use `Surface.convert()`**: Sempre converta imagens após carregá-las para melhor performance 
   ```python
   imagem = pygame.image.load('imagem.png').convert()
   ```

2. **Imagens com Transparência**: Use `convert_alpha()` 
   ```python
   imagem = pygame.image.load('imagem.png').convert_alpha()
   ```

3. **Evite operações por pixel no loop principal**: Use blit de imagens pré-renderizadas

4. **Carregue recursos uma vez**: Não carregue imagens/sons dentro do game loop 

5. **Use dirty rects quando necessário**: Para jogos muito complexos, atualize apenas partes da tela 

### 8. Constantes Importantes (pygame.locals)

```python
from pygame.locals import *

# Tipos de eventos
QUIT
KEYDOWN, KEYUP
MOUSEMOTION, MOUSEBUTTONDOWN, MOUSEBUTTONUP
ACTIVEEVENT

# Teclas
K_a, K_b, ... K_z
K_0, K_1, ... K_9
K_F1, ... K_F12
K_LEFT, K_RIGHT, K_UP, K_DOWN
K_SPACE, K_RETURN, K_ESCAPE

# Modificadores de teclas
KMOD_LSHIFT, KMOD_RSHIFT
KMOD_LCTRL, KMOD_RCTRL
KMOD_LALT, KMOD_RALT
KMOD_CAPS, KMOD_NUM

# Flags para display
FULLSCREEN  # Tela cheia
DOUBLEBUF   # Double buffer (obsoleto em versões recentes)
HWSURFACE   # Aceleração por hardware (obsoleto) 
RESIZABLE   # Janela redimensionável
NOFRAME     # Janela sem bordas
```

### Conclusão

Este manual cobre a grande maioria das funcionalidades mais utilizadas do Pygame. Para referência completa e atualizada, consulte sempre a [documentação oficial do Pygame](https://www.pygame.org/docs/) .

A melhor forma de aprender é praticando! Comece com exemplos simples e gradualmente adicione mais funcionalidades. O Pygame também inclui vários exemplos que podem ser executados com :
```bash
python -m pygame.examples.aliens
python -m pygame.examples.chimp
python -m pygame.examples.stars
```
