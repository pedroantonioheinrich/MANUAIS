# Manual Completo: Como Usar Sprites no Pygame

## Índice
1. [Introdução aos Sprites](#introdução-aos-sprites)
2. [Criando Seu Primeiro Sprite](#criando-seu-primeiro-sprite)
3. [Trabalhando com Sprite Groups](#trabalhando-com-sprite-groups)
4. [Movimentando Sprites](#movimentando-sprites)
5. [Detecção de Colisão](#detecção-de-colisão)
6. [Animações com Sprites](#animações-com-sprites)
7. [Sprite Sheets](#sprite-sheets)
8. [Boas Práticas](#boas-práticas)
9. [Projeto Prático](#projeto-prático)

---

## 1. Introdução aos Sprites

### O que são Sprites?

Em Pygame, **sprites** são objetos gráficos bidimensionais que representam elementos visuais em um jogo - como personagens, inimigos, itens ou projéteis . O termo vem dos primeiros jogos de arcade, onde hardware especializado era usado para mover rapidamente objetos gráficos na tela .

### Por que usar Sprites?

- **Organização**: Encapsulam dados (imagem, posição) e comportamentos (movimento, animação)
- **Eficiência**: Permitem desenhar e atualizar múltiplos objetos simultaneamente
- **Funcionalidades integradas**: Colisão, agrupamento e renderização otimizada

### Estrutura Básica

Todo sprite em Pygame precisa de dois atributos fundamentais :
- `self.image`: A superfície (imagem) que será desenhada na tela
- `self.rect`: O retângulo que define posição e área de colisão

---

## 2. Criando Seu Primeiro Sprite

### Configuração Inicial

```python
import pygame
import os

# Inicialização do Pygame
pygame.init()

# Configurações da tela
LARGURA, ALTURA = 800, 600
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Meu Primeiro Sprite")
clock = pygame.time.Clock()
```

### Criando uma Classe Sprite

```python
class Jogador(pygame.sprite.Sprite):
    def __init__(self, x, y):
        # Chamar construtor da classe pai
        super().__init__()
        
        # Carregar imagem do sprite
        self.image = pygame.image.load("jogador.png")
        
        # Obter retângulo da imagem para posicionamento
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        
        # Atributos adicionais
        self.velocidade = 5
```

### Criando Sprites com Formas Simples

```python
class Obstaculo(pygame.sprite.Sprite):
    def __init__(self, cor, largura, altura, x, y):
        super().__init__()
        
        # Criar superfície preenchida com cor
        self.image = pygame.Surface([largura, altura])
        self.image.fill(cor)
        
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
```

### Instanciando e Usando Sprites

```python
# Criar instâncias
jogador = Jogador(100, 100)
obstaculo1 = Obstaculo((255, 0, 0), 50, 50, 300, 200)
obstaculo2 = Obstaculo((0, 255, 0), 30, 30, 400, 300)

# Loop principal
executando = True
while executando:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            executando = False
    
    # Limpar tela
    tela.fill((255, 255, 255))
    
    # Desenhar sprites individualmente
    tela.blit(jogador.image, jogador.rect)
    tela.blit(obstaculo1.image, obstaculo1.rect)
    tela.blit(obstaculo2.image, obstaculo2.rect)
    
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```

---

## 3. Trabalhando com Sprite Groups

Os **Sprite Groups** (grupos de sprites) são contêineres que facilitam o gerenciamento de múltiplos sprites .

### Tipos de Groups

```python
# Group básico - mais comum
todos_sprites = pygame.sprite.Group()

# GroupSingle - contém apenas um sprite
jogador_group = pygame.sprite.GroupSingle()

# LayeredUpdates - permite camadas
camadas_group = pygame.sprite.LayeredUpdates()
```

### Adicionando Sprites aos Groups

```python
# Criar groups
todos_sprites = pygame.sprite.Group()
inimigos = pygame.sprite.Group()
itens = pygame.sprite.Group()

# Criar sprites
jogador = Jogador(400, 300)
inimigo1 = Inimigo(100, 100)
inimigo2 = Inimigo(200, 200)
item = Item(500, 400)

# Adicionar sprites aos groups
todos_sprites.add(jogador, inimigo1, inimigo2, item)
inimigos.add(inimigo1, inimigo2)
itens.add(item)
```

### Vantagens dos Groups

```python
# ATUALIZAÇÃO: Chama update() de todos os sprites no grupo
todos_sprites.update()

# RENDERIZAÇÃO: Desenha todos os sprites de uma vez
todos_sprites.draw(tela)

# ACESSO: Iterar sobre sprites específicos
for inimigo in inimigos:
    inimigo.mover_em_direcao_ao(jogador)

# FILTRAGEM: Sprites podem pertencer a múltiplos grupos
class Inimigo(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        # Pode ser adicionado automaticamente aos grupos
        todos_sprites.add(self)
        inimigos.add(self)
```

### Exemplo Prático com Groups

```python
# Configuração inicial
todos_sprites = pygame.sprite.Group()
inimigos = pygame.sprite.Group()
projeteis = pygame.sprite.Group()

# Criar jogador
jogador = Jogador(400, 500)
todos_sprites.add(jogador)

# Loop principal
executando = True
while executando:
    # Atualizar todos os sprites
    todos_sprites.update()
    
    # Desenhar
    tela.fill((0, 0, 0))
    todos_sprites.draw(tela)
    
    pygame.display.flip()
```

---

## 4. Movimentando Sprites

### Movimento Básico

```python
class Jogador(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.image.load("jogador.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.vel_x = 0
        self.vel_y = 0
        self.velocidade = 5
    
    def update(self):
        # Atualizar posição baseado na velocidade
        self.rect.x += self.vel_x
        self.rect.y += self.vel_y
        
        # Manter dentro da tela
        if self.rect.left < 0:
            self.rect.left = 0
        if self.rect.right > LARGURA:
            self.rect.right = LARGURA
        if self.rect.top < 0:
            self.rect.top = 0
        if self.rect.bottom > ALTURA:
            self.rect.bottom = ALTURA
```

### Controle por Teclado

```python
# No loop principal
for event in pygame.event.get():
    if event.type == pygame.KEYDOWN:
        if event.key == pygame.K_LEFT:
            jogador.vel_x = -jogador.velocidade
        elif event.key == pygame.K_RIGHT:
            jogador.vel_x = jogador.velocidade
        elif event.key == pygame.K_UP:
            jogador.vel_y = -jogador.velocidade
        elif event.key == pygame.K_DOWN:
            jogador.vel_y = jogador.velocidade
    
    if event.type == pygame.KEYUP:
        if event.key in (pygame.K_LEFT, pygame.K_RIGHT):
            jogador.vel_x = 0
        if event.key in (pygame.K_UP, pygame.K_DOWN):
            jogador.vel_y = 0
```

### Movimento com Limites de Tela

```python
def update(self):
    # Atualizar posição
    self.rect.x += self.vel_x
    self.rect.y += self.vel_y
    
    # Verificar limites da tela usando retângulo do sprite
    if self.rect.left < 0:
        self.rect.left = 0
    if self.rect.right > LARGURA:
        self.rect.right = LARGURA
    if self.rect.top < 0:
        self.rect.top = 0
    if self.rect.bottom > ALTURA:
        self.rect.bottom = ALTURA
```

### Movimento Automático (Inimigos)

```python
class Inimigo(pygame.sprite.Sprite):
    def __init__(self, x, y, direcao):
        super().__init__()
        self.image = pygame.image.load("inimigo.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.direcao = direcao  # 1 para direita, -1 para esquerda
        self.velocidade = 3
    
    def update(self):
        # Movimento automático
        self.rect.x += self.velocidade * self.direcao
        
        # Inverter direção ao atingir bordas
        if self.rect.right >= LARGURA or self.rect.left <= 0:
            self.direcao *= -1
```

---

## 5. Detecção de Colisão

Pygame oferece vários métodos para detecção de colisão entre sprites .

### Colisão entre Dois Sprites

```python
# Colisão simples entre dois sprites (baseada em retângulos)
if pygame.sprite.collide_rect(sprite1, sprite2):
    print("Colisão detectada!")

# Colisão precisa baseada em máscara (útil para formas irregulares)
if pygame.sprite.collide_mask(sprite1, sprite2):
    print("Colisão precisa!")

# Colisão circular (distância entre centros)
if pygame.sprite.collide_circle(sprite1, sprite2):
    print("Colisão circular!")
```

### Colisão entre Sprite e Grupo

```python
# Verificar se um sprite colide com qualquer sprite do grupo
if pygame.sprite.spritecollide(jogador, inimigos, False):
    print("Jogador colidiu com inimigo!")
    # Parâmetros: sprite, grupo, dokill (remover sprite colidido?)

# Remover automaticamente os sprites colididos
colisoes = pygame.sprite.spritecollide(jogador, inimigos, True)
print(f"{len(colisoes)} inimigos destruídos!")
```

### Colisão entre Grupos

```python
# Detectar colisões entre dois grupos
colisoes = pygame.sprite.groupcollide(
    projeteis, inimigos, True, True
)
# True, True: remove projétil e inimigo ao colidir

# Contar colisões
if colisoes:
    print(f"{len(colisoes)} inimigos acertados!")
```

### Exemplo Prático: Jogo de Tiro

```python
class Jogador(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.image.load("jogador.png")
        self.rect = self.image.get_rect()
        self.rect.centerx = LARGURA // 2
        self.rect.bottom = ALTURA - 20
        self.velocidade = 8
        self.tempo_ultimo_tiro = 0
        self.cooldown_tiro = 250  # milissegundos
    
    def update(self):
        # Movimento com setas
        teclas = pygame.key.get_pressed()
        if teclas[pygame.K_LEFT] and self.rect.left > 0:
            self.rect.x -= self.velocidade
        if teclas[pygame.K_RIGHT] and self.rect.right < LARGURA:
            self.rect.x += self.velocidade
        
        # Disparo com espaço
        if teclas[pygame.K_SPACE]:
            agora = pygame.time.get_ticks()
            if agora - self.tempo_ultimo_tiro > self.cooldown_tiro:
                self.atirar()
                self.tempo_ultimo_tiro = agora
    
    def atirar(self):
        projetil = Projetil(self.rect.centerx, self.rect.top)
        todos_sprites.add(projetil)
        projeteis.add(projetil)

class Projetil(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface((5, 10))
        self.image.fill((255, 255, 0))
        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.bottom = y
        self.velocidade = -10
    
    def update(self):
        self.rect.y += self.velocidade
        # Remover se sair da tela
        if self.rect.bottom < 0:
            self.kill()  # Remove do grupo

class Inimigo(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((40, 30))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, LARGURA - 40)
        self.rect.y = random.randint(20, 100)
        self.velocidade = 3
    
    def update(self):
        self.rect.y += self.velocidade
        if self.rect.top > ALTURA:
            self.kill()

# Loop principal com colisões
while executando:
    # Atualizar
    todos_sprites.update()
    
    # Verificar colisões
    # Projéteis vs Inimigos
    hits = pygame.sprite.groupcollide(projeteis, inimigos, True, True)
    for hit in hits:
        pontuacao += 10
    
    # Jogador vs Inimigos
    if pygame.sprite.spritecollide(jogador, inimigos, False):
        print("Game Over!")
        executando = False
```

---

## 6. Animações com Sprites

Animações são criadas alternando rapidamente entre diferentes imagens (frames) .

### Animação com Imagens Separadas

```python
class PersonagemAnimado(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        
        # Carregar frames da animação
        self.frames_andando = [
            pygame.image.load(f"andar_{i}.png") for i in range(4)
        ]
        self.frames_parado = [pygame.image.load("parado.png")]
        
        self.frame_atual = 0
        self.image = self.frames_parado[0]
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        
        self.ultimo_update = pygame.time.get_ticks()
        self.intervalo_animacao = 100  # ms entre frames
        self.andando = False
    
    def update(self):
        # Verificar se é hora de trocar frame
        agora = pygame.time.get_ticks()
        if agora - self.ultimo_update > self.intervalo_animacao:
            self.ultimo_update = agora
            
            # Escolher lista de frames baseado no estado
            frames = self.frames_andando if self.andando else self.frames_parado
            
            # Avançar frame
            self.frame_atual = (self.frame_atual + 1) % len(frames)
            self.image = frames[self.frame_atual]
```

### Sistema de Timer para Animações

```python
class Animacao:
    def __init__(self, frames, intervalo=100, loop=True):
        self.frames = frames
        self.intervalo = intervalo
        self.loop = loop
        self.frame_atual = 0
        self.ultimo_tempo = pygame.time.get_ticks()
        self.ativo = True
    
    def atualizar(self):
        if not self.ativo:
            return self.frames[self.frame_atual]
        
        agora = pygame.time.get_ticks()
        if agora - self.ultimo_tempo > self.intervalo:
            self.ultimo_tempo = agora
            self.frame_atual += 1
            
            if self.frame_atual >= len(self.frames):
                if self.loop:
                    self.frame_atual = 0
                else:
                    self.frame_atual = len(self.frames) - 1
                    self.ativo = False
        
        return self.frames[self.frame_atual]
    
    def reset(self):
        self.frame_atual = 0
        self.ativo = True
        self.ultimo_tempo = pygame.time.get_ticks()
```

### Usando a Classe Animacao

```python
class Heroi(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        
        # Carregar todas as animações
        frames_andar = [
            pygame.image.load(f"heroi_andar_{i}.png") for i in range(8)
        ]
        frames_pular = [
            pygame.image.load(f"heroi_pular_{i}.png") for i in range(4)
        ]
        
        self.animacoes = {
            'parado': Animacao([pygame.image.load("heroi.png")], 100),
            'andar': Animacao(frames_andar, 80),
            'pular': Animacao(frames_pular, 120, loop=False)
        }
        
        self.animacao_atual = 'parado'
        self.image = self.animacoes['parado'].frames[0]
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
    
    def update(self):
        # Atualizar animação atual
        self.image = self.animacoes[self.animacao_atual].atualizar()
    
    def mudar_animacao(self, nova_animacao):
        if nova_animacao != self.animacao_atual:
            self.animacao_atual = nova_animacao
            self.animacoes[nova_animacao].reset()
```

---

## 7. Sprite Sheets

**Sprite sheets** são imagens únicas que contêm múltiplos frames de animação .

### Estrutura de uma Sprite Sheet

```
┌─────────────────────┐
│ Frame 0 │ Frame 1   │
├─────────┼───────────┤
│ Frame 2 │ Frame 3   │
└─────────┴───────────┘
```

### Carregando e Recortando Sprite Sheets

```python
class SpriteSheet:
    def __init__(self, arquivo):
        self.sheet = pygame.image.load(arquivo).convert_alpha()
    
    def get_image(self, x, y, largura, altura):
        """Extrai um frame específico da sprite sheet"""
        # Criar superfície para o frame
        image = pygame.Surface((largura, altura), pygame.SRCALPHA)
        
        # Copiar região da sprite sheet
        image.blit(self.sheet, (0, 0), (x, y, largura, altura))
        return image
    
    def get_frames(self, linhas, colunas, largura_frame, altura_frame):
        """Extrai todos os frames da sprite sheet"""
        frames = []
        for linha in range(linhas):
            for coluna in range(colunas):
                x = coluna * largura_frame
                y = linha * altura_frame
                frame = self.get_image(x, y, largura_frame, altura_frame)
                frames.append(frame)
        return frames
```

### Exemplo Completo com Sprite Sheet

```python
class PersonagemSheet(pygame.sprite.Sprite):
    def __init__(self, x, y, arquivo_sheet):
        super().__init__()
        
        # Carregar sprite sheet
        sheet = SpriteSheet(arquivo_sheet)
        
        # Configurações da animação
        self.largura_frame = 64
        self.altura_frame = 64
        
        # Extrair frames para cada direção
        self.frames_baixo = sheet.get_frames(1, 4, 
                                            self.largura_frame, 
                                            self.altura_frame)
        self.frames_cima = sheet.get_frames(1, 4,
                                          self.largura_frame,
                                          self.altura_frame,
                                          linha=1)
        self.frames_esquerda = sheet.get_frames(1, 4,
                                               self.largura_frame,
                                               self.altura_frame,
                                               linha=2)
        self.frames_direita = sheet.get_frames(1, 4,
                                              self.largura_frame,
                                              self.altura_frame,
                                              linha=3)
        
        # Estado atual
        self.direcao = 'baixo'
        self.frame_atual = 0
        self.image = self.frames_baixo[0]
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        
        # Timer para animação
        self.ultimo_update = pygame.time.get_ticks()
        self.velocidade_animacao = 100
    
    def update(self):
        # Obter lista de frames da direção atual
        frames = getattr(self, f'frames_{self.direcao}')
        
        # Atualizar frame baseado no tempo
        agora = pygame.time.get_ticks()
        if agora - self.ultimo_update > self.velocidade_animacao:
            self.ultimo_update = agora
            self.frame_atual = (self.frame_atual + 1) % len(frames)
            self.image = frames[self.frame_atual]
    
    def mover(self, dx, dy):
        # Atualizar posição
        self.rect.x += dx
        self.rect.y += dy
        
        # Atualizar direção baseado no movimento
        if dx > 0:
            self.direcao = 'direita'
        elif dx < 0:
            self.direcao = 'esquerda'
        elif dy > 0:
            self.direcao = 'baixo'
        elif dy < 0:
            self.direcao = 'cima'
```

---

## 8. Boas Práticas

### Estrutura de Código Recomendada

```python
# main.py
import pygame
import random
from sprites import Jogador, Inimigo, Projetil

# Constantes
LARGURA, ALTURA = 800, 600
FPS = 60
PRETO = (0, 0, 0)

class Jogo:
    def __init__(self):
        pygame.init()
        self.tela = pygame.display.set_mode((LARGURA, ALTURA))
        pygame.display.set_caption("Meu Jogo")
        self.clock = pygame.time.Clock()
        self.executando = True
        self.pontuacao = 0
        
        # Grupos de sprites
        self.todos_sprites = pygame.sprite.Group()
        self.inimigos = pygame.sprite.Group()
        self.projeteis = pygame.sprite.Group()
        
        # Criar jogador
        self.jogador = Jogador()
        self.todos_sprites.add(self.jogador)
    
    def novo_inimigo(self):
        inimigo = Inimigo()
        self.todos_sprites.add(inimigo)
        self.inimigos.add(inimigo)
    
    def processar_eventos(self):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                self.executando = False
    
    def atualizar(self):
        self.todos_sprites.update()
        
        # Colisões
        hits = pygame.sprite.groupcollide(
            self.projeteis, self.inimigos, True, True
        )
        for hit in hits:
            self.pontuacao += 10
        
        if pygame.sprite.spritecollide(
            self.jogador, self.inimigos, False
        ):
            self.executando = False
    
    def desenhar(self):
        self.tela.fill(PRETO)
        self.todos_sprites.draw(self.tela)
        
        # Mostrar pontuação
        fonte = pygame.font.Font(None, 36)
        texto = fonte.render(f"Pontos: {self.pontuacao}", True, (255, 255, 255))
        self.tela.blit(texto, (10, 10))
        
        pygame.display.flip()
    
    def rodar(self):
        # Criar inimigos periodicamente
        pygame.time.set_timer(pygame.USEREVENT, 2000)
        
        while self.executando:
            self.processar_eventos()
            self.atualizar()
            self.desenhar()
            self.clock.tick(FPS)
        
        pygame.quit()

if __name__ == "__main__":
    jogo = Jogo()
    jogo.rodar()
```

### Dicas Importantes

1. **Sempre use `convert_alpha()`** para imagens com transparência:
   ```python
   self.image = pygame.image.load("sprite.png").convert_alpha()
   ```

2. **Use `pygame.time.get_ticks()` para temporizadores**, não loops com `time.sleep()` 

3. **Remova sprites que não são mais necessários** com `sprite.kill()` para liberar memória 

4. **Organize sprites em múltiplos grupos** para facilitar colisões e lógica específica

5. **Mantenha sprites dentro dos limites da tela** para evitar bugs

### Padrões de Design para Sprites

```python
# Padrão Componente - Separar comportamento
class ComponenteMovimento:
    def __init__(self, velocidade):
        self.velocidade = velocidade
    
    def mover(self, sprite, dx, dy):
        sprite.rect.x += dx * self.velocidade
        sprite.rect.y += dy * self.velocidade

class ComponenteAnimacao:
    def __init__(self, frames):
        self.frames = frames
        self.frame_atual = 0
    
    def atualizar(self, sprite):
        sprite.image = self.frames[self.frame_atual]
        self.frame_atual = (self.frame_atual + 1) % len(self.frames)

class SpriteComComponentes(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.componentes = []
        self.rect = pygame.Rect(x, y, 32, 32)
    
    def adicionar_componente(self, componente):
        self.componentes.append(componente)
    
    def update(self):
        for componente in self.componentes:
            if hasattr(componente, 'atualizar'):
                componente.atualizar(self)
```

---

## 9. Projeto Prático: Coletor de Estrelas

Vamos criar um jogo simples onde o jogador coleta estrelas enquanto desvia de obstáculos.

### Código Completo

```python
import pygame
import random
import os

# Inicialização
pygame.init()

# Constantes
LARGURA, ALTURA = 800, 600
FPS = 60
BRANCO = (255, 255, 255)
PRETO = (0, 0, 0)
AMARELO = (255, 255, 0)
VERMELHO = (255, 0, 0)
AZUL = (0, 0, 255)

# Configuração da tela
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Coletor de Estrelas")
clock = pygame.time.Clock()

# Grupos de sprites
todos_sprites = pygame.sprite.Group()
estrelas = pygame.sprite.Group()
obstaculos = pygame.sprite.Group()

class Jogador(pygame.sprite.Sprite):
    """Sprite do jogador controlado pelo teclado"""
    def __init__(self):
        super().__init__()
        # Criar imagem do jogador (um retângulo azul)
        self.image = pygame.Surface((40, 40))
        self.image.fill(AZUL)
        self.rect = self.image.get_rect()
        self.rect.centerx = LARGURA // 2
        self.rect.bottom = ALTURA - 20
        self.velocidade = 8
        
        # Atributos de animação
        self.direcao = 0  # -1: esquerda, 0: parado, 1: direita
        self.piscando = False
        self.tempo_piscar = 0
    
    def update(self):
        # Movimento com teclas
        teclas = pygame.key.get_pressed()
        if teclas[pygame.K_LEFT] and self.rect.left > 0:
            self.rect.x -= self.velocidade
            self.direcao = -1
        elif teclas[pygame.K_RIGHT] and self.rect.right < LARGURA:
            self.rect.x += self.velocidade
            self.direcao = 1
        else:
            self.direcao = 0
        
        # Efeito visual quando atingido
        if self.piscando:
            agora = pygame.time.get_ticks()
            if agora - self.tempo_piscar > 1000:  # 1 segundo
                self.piscando = False
                self.image.set_alpha(255)  # Voltar ao normal
            else:
                # Alternar transparência para efeito pisca-pisca
                alpha = 255 if (agora // 100) % 2 == 0 else 100
                self.image.set_alpha(alpha)
    
    def ser_atingido(self):
        """Chamado quando jogador colide com obstáculo"""
        self.piscando = True
        self.tempo_piscar = pygame.time.get_ticks()
        self.image.set_alpha(100)

class Estrela(pygame.sprite.Sprite):
    """Estrela que o jogador deve coletar"""
    def __init__(self):
        super().__init__()
        # Criar estrela (um círculo amarelo)
        self.image = pygame.Surface((30, 30), pygame.SRCALPHA)
        pygame.draw.circle(self.image, AMARELO, (15, 15), 15)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, LARGURA - 30)
        self.rect.y = random.randint(-100, -30)
        
        # Animação de rotação
        self.angulo = 0
        self.rotacao_velocidade = random.randint(2, 5)
    
    def update(self):
        # Cair
        self.rect.y += 3
        
        # Rotacionar
        self.angulo = (self.angulo + self.rotacao_velocidade) % 360
        centro = self.rect.center
        self.image = pygame.transform.rotate(
            pygame.Surface((30, 30), pygame.SRCALPHA), self.angulo
        )
        pygame.draw.circle(self.image, AMARELO, (15, 15), 15)
        self.rect = self.image.get_rect(center=centro)
        
        # Remover se sair da tela
        if self.rect.top > ALTURA:
            self.kill()

class Obstaculo(pygame.sprite.Sprite):
    """Obstáculo que o jogador deve evitar"""
    def __init__(self):
        super().__init__()
        # Criar obstáculo (um quadrado vermelho)
        self.image = pygame.Surface((40, 40))
        self.image.fill(VERMELHO)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, LARGURA - 40)
        self.rect.y = random.randint(-100, -50)
        self.velocidade = random.randint(2, 6)
    
    def update(self):
        self.rect.y += self.velocidade
        if self.rect.top > ALTURA:
            self.kill()

# Criar jogador
jogador = Jogador()
todos_sprites.add(jogador)

# Temporizadores para criação de objetos
pygame.time.set_timer(pygame.USEREVENT + 1, 1000)  # Estrelas
pygame.time.set_timer(pygame.USEREVENT + 2, 2000)  # Obstáculos

# Variáveis do jogo
pontuacao = 0
vidas = 3
fonte = pygame.font.Font(None, 36)
game_over = False

# Loop principal
executando = True
while executando:
    # Processar eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            executando = False
        
        # Criar estrelas
        elif event.type == pygame.USEREVENT + 1 and not game_over:
            estrela = Estrela()
            todos_sprites.add(estrela)
            estrelas.add(estrela)
        
        # Criar obstáculos
        elif event.type == pygame.USEREVENT + 2 and not game_over:
            obstaculo = Obstaculo()
            todos_sprites.add(obstaculo)
            obstaculos.add(obstaculo)
    
    if not game_over:
        # Atualizar sprites
        todos_sprites.update()
        
        # Verificar colisões com estrelas
        estrelas_coletadas = pygame.sprite.spritecollide(jogador, estrelas, True)
        pontuacao += len(estrelas_coletadas) * 10
        
        # Verificar colisões com obstáculos
        colisoes_obstaculos = pygame.sprite.spritecollide(jogador, obstaculos, True)
        if colisoes_obstaculos and not jogador.piscando:
            vidas -= 1
            jogador.ser_atingido()
            if vidas <= 0:
                game_over = True
        
        # Verificar se coletou todas as estrelas (vitória)
        if pontuacao >= 200:
            game_over = True
    
    # Desenhar
    tela.fill(PRETO)
    todos_sprites.draw(tela)
    
    # Mostrar pontuação e vidas
    texto_pontos = fonte.render(f"Pontos: {pontuacao}", True, BRANCO)
    tela.blit(texto_pontos, (10, 10))
    
    texto_vidas = fonte.render(f"Vidas: {vidas}", True, BRANCO)
    tela.blit(texto_vidas, (10, 50))
    
    # Mostrar mensagem de game over
    if game_over:
        if vidas > 0:
            texto = "VITÓRIA! Você coletou todas as estrelas!"
        else:
            texto = "GAME OVER!"
        
        texto_fim = fonte.render(texto, True, BRANCO)
        texto_rect = texto_fim.get_rect(center=(LARGURA//2, ALTURA//2))
        tela.blit(texto_fim, texto_rect)
        
        texto_reiniciar = fonte.render("Pressione R para reiniciar", True, BRANCO)
        texto_reiniciar_rect = texto_reiniciar.get_rect(center=(LARGURA//2, ALTURA//2 + 50))
        tela.blit(texto_reiniciar, texto_reiniciar_rect)
        
        # Verificar tecla R para reiniciar
        teclas = pygame.key.get_pressed()
        if teclas[pygame.K_r]:
            # Reiniciar jogo
            todos_sprites.empty()
            estrelas.empty()
            obstaculos.empty()
            
            jogador = Jogador()
            todos_sprites.add(jogador)
            
            pontuacao = 0
            vidas = 3
            game_over = False
    
    pygame.display.flip()
    clock.tick(FPS)

pygame.quit()
```

