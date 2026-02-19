# Manual Completo para Criação de Jogos de Terminal em Python

## Índice
1. [Introdução](#introducao)
2. [Configuração do Ambiente](#configuracao)
3. [Entrada do Usuário](#entrada-usuario)
4. [Exibição e Formatação](#exibicao)
5. [Estruturas de Dados para Jogos](#estruturas)
6. [Game Loop](#game-loop)
7. [Jogos Baseados em Texto](#jogos-texto)
8. [Jogos com Interface Visual no Terminal](#jogos-visuais)
9. [Frameworks e Bibliotecas](#frameworks)
10. [Exemplos Práticos](#exemplos)
11. [Dicas e Boas Práticas](#dicas)

---

## 1. Introdução {#introducao}

Jogos de terminal são uma excelente forma de aprender programação, praticar lógica e criar projetos divertidos sem a complexidade de interfaces gráficas. Eles rodam diretamente no console/terminal e podem variar desde simples jogos de texto até experiências visuais complexas usando caracteres ASCII.

**Vantagens:**
- ✅ Curva de aprendizado suave
- ✅ Foco na lógica do jogo
- ✅ Portabilidade (funcionam em qualquer sistema)
- ✅ Ótimos para prototipagem

---

## 2. Configuração do Ambiente {#configuracao}

### 2.1 Estrutura Básica de um Projeto

```
meu_jogo/
├── main.py
├── config.py
├── utils.py
├── game_data/
│   ├── save.json
│   └── levels.json
└── README.md
```

### 2.2 Configuração Inicial

```python
# config.py
import os
import sys

# Configurações do terminal
def configurar_terminal():
    """Configura o terminal para melhor experiência de jogo"""
    if os.name == 'nt':  # Windows
        os.system('cls')
        os.system('mode con: cols=80 lines=24')
    else:  # Linux/Mac
        os.system('clear')
        sys.stdout.write("\x1b[8;24;80t")  # Redimensiona terminal
```

---

## 3. Entrada do Usuário {#entrada-usuario}

### 3.1 Captura Básica com input()

```python
# Entrada simples
nome = input("Digite seu nome: ")
print(f"Bem-vindo, {nome}!")

# Entrada numérica com validação
def obter_numero(mensagem, minimo=1, maximo=10):
    while True:
        try:
            valor = int(input(mensagem))
            if minimo <= valor <= maximo:
                return valor
            print(f"Digite um número entre {minimo} e {maximo}")
        except ValueError:
            print("Digite um número válido")

idade = obter_numero("Qual sua idade? ", 1, 120)
```

### 3.2 Captura de Tecla Única (sem Enter)

```python
# utils.py
import sys
import tty
import termios

def getch() -> str:
    """Captura um único caractere sem precisar de Enter"""
    try:
        # Para Windows
        import msvcrt
        return msvcrt.getch().decode('utf-8')
    except ImportError:
        # Para Unix/Linux/Mac
        fd = sys.stdin.fileno()
        old_settings = termios.tcgetattr(fd)
        try:
            tty.setraw(fd)
            ch = sys.stdin.read(1)
        finally:
            termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
        return ch

# Exemplo de uso
print("Pressione WASD para mover (q para sair)")
while True:
    tecla = getch().lower()
    if tecla == 'w':
        print("↑ Cima")
    elif tecla == 's':
        print("↓ Baixo")
    elif tecla == 'a':
        print("← Esquerda")
    elif tecla == 'd':
        print("→ Direita")
    elif tecla == 'q':
        print("Saindo...")
        break
```

### 3.3 Mapeamento de Teclas

```python
# Definindo ações do jogo
class Acoes:
    CIMA = 'w'
    BAIXO = 's'
    ESQUERDA = 'a'
    DIREITA = 'd'
    ATACAR = ' '
    PAUSAR = 'p'
    SAIR = 'q'

    @classmethod
    def executar(cls, tecla):
        acoes = {
            cls.CIMA: mover_cima,
            cls.BAIXO: mover_baixo,
            cls.ESQUERDA: mover_esquerda,
            cls.DIREITA: mover_direita,
            cls.ATACAR: atacar,
            cls.PAUSAR: pausar_jogo,
            cls.SAIR: sair_jogo
        }
        if tecla in acoes:
            return acoes[tecla]()
        return None
```

### 3.4 Menus Interativos

```python
def menu_principal():
    """Menu navegável com setas"""
    opcoes = ["Novo Jogo", "Carregar Jogo", "Opções", "Sair"]
    selecionado = 0
    
    while True:
        os.system('cls' if os.name == 'nt' else 'clear')
        print("=== JOGO RPG ===\n")
        
        for i, opcao in enumerate(opcoes):
            if i == selecionado:
                print(f"> {opcao} <")  # Opção selecionada
            else:
                print(f"  {opcao}")
        
        print("\nUse W/S para navegar, Enter para selecionar")
        
        tecla = getch().lower()
        if tecla == 'w' and selecionado > 0:
            selecionado -= 1
        elif tecla == 's' and selecionado < len(opcoes) - 1:
            selecionado += 1
        elif tecla == '\r' or tecla == '\n':  # Enter
            return selecionado
```

---

## 4. Exibição e Formatação {#exibicao}

### 4.1 Cores com Colorama

```python
# Instalação: pip install colorama
from colorama import init, Fore, Back, Style

# Inicializa o colorama (necessário para Windows)
init(autoreset=True)

# Cores disponíveis
print(Fore.RED + "Texto vermelho")
print(Fore.GREEN + "Texto verde")
print(Fore.BLUE + "Texto azul")
print(Fore.YELLOW + "Texto amarelo")
print(Fore.MAGENTA + "Texto magenta")
print(Fore.CYAN + "Texto ciano")
print(Fore.WHITE + "Texto branco")

# Cores de fundo
print(Back.RED + "Fundo vermelho")
print(Back.GREEN + "Fundo verde")
print(Back.BLUE + "Fundo azul")

# Estilos
print(Style.BRIGHT + "Texto em negrito")
print(Style.DIM + "Texto escurecido")
print(Style.NORMAL + "Texto normal")

# Reset
print(Fore.RED + "Vermelho" + Style.RESET_ALL + " Normal")
```

### 4.2 Formatação de Texto

```python
# Centralizar texto
def centralizar(texto, largura=80):
    espacos = (largura - len(texto)) // 2
    return " " * espacos + texto

# Criar bordas
def criar_janela(titulo, conteudo, largura=60):
    linha_superior = "┌" + "─" * (largura - 2) + "┐"
    linha_titulo = "│" + centralizar(titulo, largura - 2) + "│"
    linha_vazia = "│" + " " * (largura - 2) + "│"
    linha_inferior = "└" + "─" * (largura - 2) + "┘"
    
    resultado = [linha_superior, linha_titulo, linha_vazia]
    
    # Adiciona linhas de conteúdo
    for linha in conteudo.split('\n'):
        linha_formatada = "│ " + linha.ljust(largura - 4) + " │"
        resultado.append(linha_formatada)
    
    resultado.append(linha_vazia)
    resultado.append(linha_inferior)
    
    return '\n'.join(resultado)

# Uso
conteudo = """Bem-vindo ao jogo!
Você está em uma masmorra escura.
Encontre a saída antes que seja tarde demais."""
print(criar_janela("Aventura", conteudo))
```

### 4.3 Animações Simples

```python
import time
import sys

def animacao_carregamento(segundos=2):
    """Animação de carregamento com spinner"""
    spin = ['|', '/', '-', '\\']
    for i in range(20):
        sys.stdout.write(f"\rCarregando... {spin[i % 4]}")
        sys.stdout.flush()
        time.sleep(segundos / 20)
    sys.stdout.write("\r" + " " * 20 + "\r")

def escrever_devagar(texto, velocidade=0.05):
    """Escreve texto caracter por caracter"""
    for char in texto:
        sys.stdout.write(char)
        sys.stdout.flush()
        time.sleep(velocidade)
    print()

# Exemplo
escrever_devagar("Em uma galáxia muito, muito distante...")
```

### 4.4 Barras de Progresso

```python
def barra_progresso(valor, maximo, largura=20):
    """Cria uma barra de progresso visual"""
    percentual = valor / maximo
    preenchido = int(largura * percentual)
    vazio = largura - preenchido
    
    barra = "█" * preenchido + "░" * vazio
    return f"[{barra}] {percentual:.1%}"

# Em combate
vida = 45
vida_max = 100
print(f"Vida: {barra_progresso(vida, vida_max)}")
```

---

## 5. Estruturas de Dados para Jogos {#estruturas}

### 5.1 Classe Jogador

```python
class Jogador:
    def __init__(self, nome):
        self.nome = nome
        self.nivel = 1
        self.vida = 100
        self.vida_max = 100
        self.mana = 50
        self.mana_max = 50
        self.xp = 0
        self.xp_proximo_nivel = 100
        self.inventario = []
        self.ouro = 0
        self.posicao = (0, 0)
    
    def ganhar_xp(self, quantidade):
        self.xp += quantidade
        if self.xp >= self.xp_proximo_nivel:
            self.subir_nivel()
    
    def subir_nivel(self):
        self.nivel += 1
        self.xp -= self.xp_proximo_nivel
        self.xp_proximo_nivel = int(self.xp_proximo_nivel * 1.5)
        self.vida_max += 20
        self.vida = self.vida_max
        self.mana_max += 10
        self.mana = self.mana_max
        print(f"{self.nome} subiu para o nível {self.nivel}!")
    
    def tomar_dano(self, dano):
        self.vida -= dano
        if self.vida < 0:
            self.vida = 0
        return self.vida > 0
    
    def curar(self, quantidade):
        self.vida = min(self.vida + quantidade, self.vida_max)
    
    def __str__(self):
        return (f"{self.nome} (Nv.{self.nivel})\n"
                f"Vida: {self.vida}/{self.vida_max}\n"
                f"Mana: {self.mana}/{self.mana_max}\n"
                f"XP: {self.xp}/{self.xp_proximo_nivel}")
```

### 5.2 Classe Inimigo

```python
import random

class Inimigo:
    def __init__(self, nome, nivel):
        self.nome = nome
        self.nivel = nivel
        self.vida = 30 + (nivel * 15)
        self.vida_max = self.vida
        self.dano = 5 + (nivel * 3)
        self.xp_recompensa = 20 * nivel
        self.ouro_recompensa = 5 * nivel
    
    def atacar(self):
        return random.randint(int(self.dano * 0.8), int(self.dano * 1.2))
    
    def esta_vivo(self):
        return self.vida > 0
    
    def __str__(self):
        return f"{self.nome} (Nv.{self.nivel}) - Vida: {self.vida}/{self.vida_max}"
```

### 5.3 Classe Item

```python
class Item:
    def __init__(self, nome, tipo, valor, efeito=None):
        self.nome = nome
        self.tipo = tipo  # 'arma', 'pocao', 'armadura', 'chave'
        self.valor = valor
        self.efeito = efeito  # função que será executada ao usar
    
    def usar(self, jogador):
        if self.efeito:
            self.efeito(jogador)
        elif self.tipo == 'pocao':
            jogador.curar(self.valor)
        
        return f"Usou {self.nome}"

# Criando itens
def pocao_curar(jogador):
    jogador.curar(30)

pocao = Item("Poção de Cura", "pocao", 30, pocao_curar)
espada = Item("Espada Longa", "arma", 15, None)
```

---

## 6. Game Loop {#game-loop}

### 6.1 Estrutura Básica do Game Loop

```python
import time

class Game:
    def __init__(self):
        self.jogador = None
        self.running = True
        self.paused = False
        self.frame_count = 0
        self.last_time = time.time()
    
    def iniciar(self):
        """Inicializa o jogo"""
        print("Bem-vindo ao jogo!")
        nome = input("Digite seu nome: ")
        self.jogador = Jogador(nome)
        self.loop_principal()
    
    def processar_entrada(self):
        """Processa entrada do usuário"""
        tecla = getch().lower()
        
        if tecla == 'p':
            self.paused = not self.paused
        elif tecla == 'q':
            self.running = False
        elif not self.paused:
            # Processa outras ações
            if tecla == 'w':
                self.mover_jogador(0, -1)
            elif tecla == 's':
                self.mover_jogador(0, 1)
            elif tecla == 'a':
                self.mover_jogador(-1, 0)
            elif tecla == 'd':
                self.mover_jogador(1, 0)
    
    def atualizar(self, delta_time):
        """Atualiza estado do jogo"""
        if not self.paused:
            self.frame_count += 1
            # Atualiza inimigos, física, etc.
    
    def renderizar(self):
        """Renderiza o jogo"""
        os.system('cls' if os.name == 'nt' else 'clear')
        print(f"FPS: {1/delta_time:.1f}" if 'delta_time' in locals() else "")
        print(self.jogador)
        # Renderiza o resto do jogo
    
    def loop_principal(self):
        """Game loop principal"""
        while self.running:
            current_time = time.time()
            delta_time = current_time - self.last_time
            self.last_time = current_time
            
            self.processar_entrada()
            self.atualizar(delta_time)
            self.renderizar()
            
            # Controla FPS
            time.sleep(0.016)  ~60 FPS
        
        print("Fim de jogo!")
```

### 6.2 Sistema de Estados

```python
from enum import Enum

class GameState(Enum):
    MENU = 1
    JOGANDO = 2
    PAUSADO = 3
    COMBATE = 4
    GAME_OVER = 5
    VITORIA = 6

class GameWithStates:
    def __init__(self):
        self.state = GameState.MENU
        self.previous_state = None
    
    def change_state(self, new_state):
        self.previous_state = self.state
        self.state = new_state
        self.on_state_enter(new_state)
    
    def on_state_enter(self, state):
        """Chamado ao entrar em um novo estado"""
        if state == GameState.COMBATE:
            self.iniciar_combate()
        elif state == GameState.GAME_OVER:
            self.mostrar_game_over()
    
    def loop(self):
        while True:
            if self.state == GameState.MENU:
                self.processar_menu()
            elif self.state == GameState.JOGANDO:
                self.processar_jogo()
            elif self.state == GameState.PAUSADO:
                self.processar_pausa()
            elif self.state == GameState.COMBATE:
                self.processar_combate()
            elif self.state == GameState.GAME_OVER:
                self.processar_game_over()
                break
```

---

## 7. Jogos Baseados em Texto {#jogos-texto}

### 7.1 Jogo de Aventura Textual (Escolha sua Aventura)

```python
class TextAdventure:
    def __init__(self):
        self.game_tree = {
            "inicio": {
                "message": "Você está em uma floresta escura. Há dois caminhos à sua frente.",
                "choices": {
                    "A": ["Seguir pelo caminho da esquerda", "caverna"],
                    "B": ["Seguir pelo caminho da direita", "rio"],
                    "C": ["Ficar onde está", "fim_triste"]
                }
            },
            "caverna": {
                "message": "Você entra em uma caverna escura. Ouve um rugido.",
                "choices": {
                    "A": ["Investigar o rugido", "dragão"],
                    "B": ["Sair correndo", "inicio"]
                }
            },
            "rio": {
                "message": "Você chega a um rio cristalino. Há uma canoa.",
                "choices": {
                    "A": ["Atravessar de canoa", "tesouro"],
                    "B": ["Seguir o rio", "cachoeira"]
                }
            },
            "dragão": {
                "message": "É um dragão! Ele cospe fogo. Fim da sua jornada.",
                "choices": {}
            },
            "tesouro": {
                "message": "Você encontra um baú de tesouro! Parabéns, você venceu!",
                "choices": {}
            },
            "cachoeira": {
                "message": "Você cai de uma cachoeira. Fim da jornada.",
                "choices": {}
            },
            "fim_triste": {
                "message": "Você ficou parado até escurecer. Os lobos apareceram...",
                "choices": {}
            }
        }
    
    def jogar(self):
        current_state = "inicio"
        
        while True:
            state = self.game_tree[current_state]
            
            # Limpa a tela
            os.system('cls' if os.name == 'nt' else 'clear')
            
            # Mostra mensagem
            print("\n" + "="*60)
            print(centralizar("AVENTURA NA FLORESTA", 60))
            print("="*60 + "\n")
            print(state["message"] + "\n")
            
            # Mostra escolhas
            if len(state["choices"]) == 0:
                print("\nFIM DE JOGO")
                input("\nPressione Enter para sair...")
                break
            
            for key, value in state["choices"].items():
                print(f"{key}: {value[0]}")
            
            # Processa escolha
            choice = input("\nEscolha uma opção: ").upper()
            
            if choice in state["choices"]:
                current_state = state["choices"][choice][1]
            else:
                print("Opção inválida!")
                time.sleep(1)
```

### 7.2 RPG de Terminal

```python
class TerminalRPG:
    def __init__(self):
        self.jogador = None
        self.local_atual = "vila"
        self.locais = {
            "vila": {
                "nome": "Vila de Início",
                "descricao": "Uma pequena vila pacífica.",
                "acoes": ["Loja", "Floresta", "Estátus", "Descansar"],
                "inimigos": []
            },
            "floresta": {
                "nome": "Floresta Sombria",
                "descricao": "Árvores densas bloqueiam a luz do sol.",
                "acoes": ["Explorar", "Voltar", "Caçar"],
                "inimigos": ["Goblin", "Lobo", "Esqueleto"]
            },
            "masmorra": {
                "nome": "Masmorra Antiga",
                "descricao": "Pedras úmidas e um cheiro de mofo.",
                "acoes": ["Explorar", "Voltar", "Procurar tesouro"],
                "inimigos": ["Zumbi", "Fantasma", "Esqueleto Guerreiro"]
            }
        }
    
    def iniciar(self):
        print("=== TERMINAL RPG ===\n")
        nome = input("Nome do herói: ")
        self.jogador = Jogador(nome)
        self.loop_principal()
    
    def loop_principal(self):
        while self.jogador.vida > 0:
            self.mostrar_local()
            self.processar_acao()
    
    def mostrar_local(self):
        local = self.locais[self.local_atual]
        print("\n" + "="*50)
        print(centralizar(f"📍 {local['nome']}", 50))
        print("="*50)
        print(f"\n{local['descricao']}\n")
        print(self.jogador)
        print("\nAções disponíveis:")
        
        for i, acao in enumerate(local['acoes'], 1):
            print(f"{i}. {acao}")
    
    def processar_acao(self):
        try:
            escolha = int(input("\nEscolha: ")) - 1
            acao = self.locais[self.local_atual]['acoes'][escolha]
            
            if acao == "Loja":
                self.loja()
            elif acao == "Floresta":
                self.local_atual = "floresta"
            elif acao == "Voltar":
                self.local_atual = "vila"
            elif acao == "Explorar":
                self.encontrar_inimigo()
            elif acao == "Estátus":
                input("Pressione Enter para continuar...")
            elif acao == "Descansar":
                self.jogador.curar(20)
                print("Você descansou e recuperou 20 de vida!")
                time.sleep(1)
            elif acao == "Caçar":
                self.cacar()
        except (ValueError, IndexError):
            print("Ação inválida!")
            time.sleep(1)
    
    def encontrar_inimigo(self):
        import random
        local = self.locais[self.local_atual]
        if local['inimigos']:
            inimigo_nome = random.choice(local['inimigos'])
            inimigo = Inimigo(inimigo_nome, self.jogador.nivel)
            self.combate(inimigo)
    
    def combate(self, inimigo):
        print(f"\n⚔️  Um {inimigo.nome} apareceu!\n")
        
        while inimigo.esta_vivo() and self.jogador.vida > 0:
            print(inimigo)
            print(self.jogador)
            print("\n1. Atacar\n2. Fugir")
            
            escolha = input("\nEscolha: ")
            
            if escolha == "1":
                # Jogador ataca
                dano = random.randint(10, 20)
                inimigo.vida -= dano
                print(f"Você causou {dano} de dano!")
                
                if inimigo.esta_vivo():
                    # Inimigo ataca
                    dano = inimigo.atacar()
                    vivo = self.jogador.tomar_dano(dano)
                    print(f"{inimigo.nome} causou {dano} de dano!")
                    
                    if not vivo:
                        print("\n💀 VOCÊ MORREU!")
                        return
            elif escolha == "2":
                if random.random() > 0.5:
                    print("Você conseguiu fugir!")
                    return
                else:
                    print("Não conseguiu fugir!")
                    dano = inimigo.atacar()
                    self.jogador.tomar_dano(dano)
            
            time.sleep(1)
            os.system('cls' if os.name == 'nt' else 'clear')
        
        if self.jogador.vida > 0:
            print(f"\n🏆 Você derrotou {inimigo.nome}!")
            self.jogador.ganhar_xp(inimigo.xp_recompensa)
            self.jogador.ouro += inimigo.ouro_recompensa
    
    def loja(self):
        print("\n=== LOJA ===")
        print("1. Poção de Cura - 10 ouro")
        print("2. Espada - 50 ouro")
        print("3. Armadura - 40 ouro")
        print("4. Sair")
        
        escolha = input("\nEscolha: ")
        
        if escolha == "1" and self.jogador.ouro >= 10:
            self.jogador.ouro -= 10
            pocao = Item("Poção de Cura", "pocao", 30, None)
            self.jogador.inventario.append(pocao)
            print("Você comprou uma poção!")
        # ... outros itens
```

---

## 8. Jogos com Interface Visual no Terminal {#jogos-visuais}

### 8.1 Jogo da Velha com Cores

```python
from colorama import init, Fore, Back, Style

init(autoreset=True)

class JogoDaVelha:
    def __init__(self):
        self.tabuleiro = [' ' for _ in range(9)]
        self.jogador_atual = 'X'
        self.jogador_cores = {'X': Fore.RED, 'O': Fore.BLUE}
    
    def mostrar_tabuleiro(self):
        print("\n" + " " * 10 + "JOGO DA VELHA\n")
        
        for i in range(0, 9, 3):
            linha = []
            for j in range(3):
                pos = i + j
                if self.tabuleiro[pos] == ' ':
                    linha.append(Fore.WHITE + str(pos))
                else:
                    cor = self.jogador_cores[self.tabuleiro[pos]]
                    linha.append(cor + f" {self.tabuleiro[pos]} ")
            
            print(" " * 10 + " │ ".join(linha))
            
            if i < 6:
                print(" " * 10 + "─────────┼─────────┼─────────")
        
        print()
    
    def fazer_jogada(self, posicao):
        if self.tabuleiro[posicao] == ' ':
            self.tabuleiro[posicao] = self.jogador_atual
            return True
        return False
    
    def verificar_vitoria(self):
        combinacoes = [
            [0,1,2], [3,4,5], [6,7,8],  # Linhas
            [0,3,6], [1,4,7], [2,5,8],  # Colunas
            [0,4,8], [2,4,6]             # Diagonais
        ]
        
        for combo in combinacoes:
            if (self.tabuleiro[combo[0]] == self.tabuleiro[combo[1]] == 
                self.tabuleiro[combo[2]] != ' '):
                return self.tabuleiro[combo[0]]
        return None
    
    def jogar(self):
        for _ in range(9):
            self.mostrar_tabuleiro()
            
            cor = self.jogador_cores[self.jogador_atual]
            print(f"Vez do jogador {cor}{self.jogador_atual}{Style.RESET_ALL}")
            
            try:
                pos = int(input("Escolha uma posição (0-8): "))
                if pos < 0 or pos > 8:
                    print(Fore.RED + "Posição inválida!")
                    continue
                
                if self.fazer_jogada(pos):
                    vencedor = self.verificar_vitoria()
                    if vencedor:
                        self.mostrar_tabuleiro()
                        cor = self.jogador_cores[vencedor]
                        print(f"\n{cor}Jogador {vencedor} venceu!{Style.RESET_ALL}")
                        return
                    
                    self.jogador_atual = 'O' if self.jogador_atual == 'X' else 'X'
                else:
                    print(Fore.RED + "Posição já ocupada!")
            
            except ValueError:
                print(Fore.RED + "Digite um número válido!")
        
        self.mostrar_tabuleiro()
        print(Fore.YELLOW + "\nEmpate!")

# Jogar
if __name__ == "__main__":
    jogo = JogoDaVelha()
    jogo.jogar()
```

### 8.2 Jogo Campo Minado no Terminal

```python
import random
import os

class CampoMinado:
    def __init__(self, tamanho=8, bombas=10):
        self.tamanho = tamanho
        self.bombas = bombas
        self.tabuleiro = [[' ' for _ in range(tamanho)] for _ in range(tamanho)]
        self.revelado = [[False for _ in range(tamanho)] for _ in range(tamanho)]
        self.bandeiras = [[False for _ in range(tamanho)] for _ in range(tamanho)]
        self.jogo_ativo = True
        self.colocar_bombas()
        self.calcular_numeros()
    
    def colocar_bombas(self):
        bombas_colocadas = 0
        while bombas_colocadas < self.bombas:
            x = random.randint(0, self.tamanho - 1)
            y = random.randint(0, self.tamanho - 1)
            if self.tabuleiro[y][x] != '💣':
                self.tabuleiro[y][x] = '💣'
                bombas_colocadas += 1
    
    def calcular_numeros(self):
        for y in range(self.tamanho):
            for x in range(self.tamanho):
                if self.tabuleiro[y][x] == '💣':
                    continue
                
                bombas_vizinhas = 0
                for dy in [-1, 0, 1]:
                    for dx in [-1, 0, 1]:
                        if dx == 0 and dy == 0:
                            continue
                        ny, nx = y + dy, x + dx
                        if 0 <= ny < self.tamanho and 0 <= nx < self.tamanho:
                            if self.tabuleiro[ny][nx] == '💣':
                                bombas_vizinhas += 1
                
                if bombas_vizinhas > 0:
                    self.tabuleiro[y][x] = str(bombas_vizinhas)
    
    def mostrar(self):
        os.system('cls' if os.name == 'nt' else 'clear')
        print("\n   CAMPO MINADO\n")
        
        # Mostra números das colunas
        print("   " + " ".join([str(i) for i in range(self.tamanho)]))
        
        for y in range(self.tamanho):
            print(f"{y}  ", end="")
            for x in range(self.tamanho):
                if self.revelado[y][x]:
                    if self.tabuleiro[y][x] == '💣':
                        print(Fore.RED + "💣", end=" ")
                    else:
                        cor = Fore.GREEN if self.tabuleiro[y][x] == ' ' else Fore.YELLOW
                        print(cor + self.tabuleiro[y][x], end=" ")
                elif self.bandeiras[y][x]:
                    print(Fore.CYAN + "🚩", end=" ")
                else:
                    print(Fore.WHITE + "⬜", end=" ")
            print(Style.RESET_ALL)
        print()
    
    def revelar(self, x, y):
        if not (0 <= x < self.tamanho and 0 <= y < self.tamanho):
            return
        if self.revelado[y][x] or self.bandeiras[y][x]:
            return
        
        self.revelado[y][x] = True
        
        if self.tabuleiro[y][x] == '💣':
            self.jogo_ativo = False
            return
        
        if self.tabuleiro[y][x] == ' ':
            # Revela células vizinhas
            for dy in [-1, 0, 1]:
                for dx in [-1, 0, 1]:
                    self.revelar(x + dx, y + dy)
    
    def jogar(self):
        while self.jogo_ativo:
            self.mostrar()
            print("Comandos: 'r x y' para revelar, 'f x y' para bandeira")
            cmd = input("> ").split()
            
            if len(cmd) != 3:
                continue
            
            acao, x, y = cmd[0], int(cmd[1]), int(cmd[2])
            
            if acao == 'r' and not self.bandeiras[y][x]:
                self.revelar(x, y)
            elif acao == 'f':
                self.bandeiras[y][x] = not self.bandeiras[y][x]
            
            # Verifica vitória
            celulas_reveladas = sum(sum(linha) for linha in self.revelado)
            if celulas_reveladas == (self.tamanho * self.tamanho) - self.bombas:
                self.mostrar()
                print(Fore.GREEN + "\n🎉 PARABÉNS! Você venceu!")
                return
        
        self.mostrar()
        print(Fore.RED + "\n💥 BOOM! Game Over!")
```

### 8.3 Jogo da Cobrinha (Snake)

```python
import os
import time
import random
from getch import getch  # Usando função definida anteriormente

class Snake:
    def __init__(self, largura=20, altura=10):
        self.largura = largura
        self.altura = altura
        self.cobra = [(altura//2, largura//2)]
        self.direcao = (0, 1)  # (dy, dx) - inicialmente direita
        self.comida = self.gerar_comida()
        self.pontos = 0
        self.jogo_ativo = True
    
    def gerar_comida(self):
        while True:
            comida = (random.randint(0, self.altura-1), 
                     random.randint(0, self.largura-1))
            if comida not in self.cobra:
                return comida
    
    def mover(self):
        cabeca = self.cobra[0]
        nova_cabeca = (cabeca[0] + self.direcao[0], 
                       cabeca[1] + self.direcao[1])
        
        # Verifica colisão com paredes
        if (nova_cabeca[0] < 0 or nova_cabeca[0] >= self.altura or
            nova_cabeca[1] < 0 or nova_cabeca[1] >= self.largura):
            self.jogo_ativo = False
            return
        
        # Verifica colisão com o próprio corpo
        if nova_cabeca in self.cobra:
            self.jogo_ativo = False
            return
        
        self.cobra.insert(0, nova_cabeca)
        
        # Verifica se comeu comida
        if nova_cabeca == self.comida:
            self.pontos += 1
            self.comida = self.gerar_comida()
        else:
            self.cobra.pop()
    
    def mudar_direcao(self, tecla):
        direcoes = {
            'w': (-1, 0),  # cima
            's': (1, 0),   # baixo
            'a': (0, -1),  # esquerda
            'd': (0, 1)    # direita
        }
        
        if tecla in direcoes:
            nova_direcao = direcoes[tecla]
            # Impede movimento na direção oposta
            if (nova_direcao[0] != -self.direcao[0] or 
                nova_direcao[1] != -self.direcao[1]):
                self.direcao = nova_direcao
    
    def mostrar(self):
        os.system('cls' if os.name == 'nt' else 'clear')
        
        # Linha superior
        print("┌" + "─" * self.largura + "┐")
        
        for y in range(self.altura):
            print("│", end="")
            for x in range(self.largura):
                if (y, x) == self.cobra[0]:
                    print(Fore.GREEN + "●" + Style.RESET_ALL, end="")
                elif (y, x) in self.cobra[1:]:
                    print(Fore.GREEN + "○" + Style.RESET_ALL, end="")
                elif (y, x) == self.comida:
                    print(Fore.RED + "★" + Style.RESET_ALL, end="")
                else:
                    print(" ", end="")
            print("│")
        
        # Linha inferior
        print("└" + "─" * self.largura + "┘")
        print(f"\nPontos: {self.pontos}")
        print("Use WASD para mover, Q para sair")
    
    def jogar(self):
        while self.jogo_ativo:
            self.mostrar()
            
            tecla = getch().lower()
            if tecla == 'q':
                break
            
            self.mudar_direcao(tecla)
            self.mover()
            time.sleep(0.1)
        
        print(Fore.YELLOW + f"\nGame Over! Pontuação: {self.pontos}")
```

---

## 9. Frameworks e Bibliotecas {#frameworks}

### 9.1 PyTextGame

```python
# Instalação: pip install pytextgame
from pytextgame.main import *

# Framework para desenvolvimento rápido de jogos de texto
# Inclui componentes prontos para RPGs, aventuras, etc.
```

### 9.2 pytermgame

```python
# Instalação: pip install pytermgame
import pytermgame as ptg

# Framework inspirado no Pygame, mas para terminal
# Suporta sprites, colisões, cenas, etc.

class MeuJogo(ptg.Game):
    def setup(self):
        self.scene = ptg.Scene()
        self.jogador = ptg.Sprite("😀", position=(10, 5))
        self.scene.add(self.jogador)
    
    def update(self):
        if self.is_key_just_pressed('w'):
            self.jogador.y -= 1
        # ...

jogo = MeuJogo()
jogo.run()
```

### 9.3 curses

```python
import curses

# Biblioteca padrão do Python para interfaces no terminal
def main(stdscr):
    curses.curs_set(0)  # Esconde o cursor
    stdscr.nodelay(1)   # Não bloqueia no getch
    stdscr.timeout(100) # Timeout para getch
    
    altura, largura = stdscr.getmaxyx()
    jogador_x = largura // 2
    jogador_y = altura // 2
    
    while True:
        stdscr.clear()
        stdscr.addstr(jogador_y, jogador_x, "@")
        stdscr.refresh()
        
        tecla = stdscr.getch()
        if tecla == ord('q'):
            break
        elif tecla == ord('w'):
            jogador_y -= 1
        elif tecla == ord('s'):
            jogador_y += 1
        elif tecla == ord('a'):
            jogador_x -= 1
        elif tecla == ord('d'):
            jogador_x += 1

curses.wrapper(main)
```

### 9.4 TextWorld (Microsoft)

```python
# Instalação: pip install textworld
import textworld

# Framework da Microsoft para criar e jogar jogos de texto
# Usado para pesquisa em IA e aprendizado por reforço

infos = textworld.EnvInfos(
    feedback=True,
    description=True,
    inventory=True
)

env = textworld.start("./meu_jogo.ulx", request_infos=infos)
game_state = env.reset()
```

---

## 10. Exemplos Práticos {#exemplos}

### 10.1 Jogo de Adivinhação

```python
import random
from colorama import Fore, Style

class JogoAdivinhacao:
    def __init__(self):
        self.numero_secreto = random.randint(1, 100)
        self.tentativas = 0
        self.max_tentativas = 10
        self.historico = []
    
    def jogar(self):
        print(Fore.CYAN + "\n" + "="*50)
        print(centralizar("JOGO DA ADIVINHAÇÃO", 50))
        print("="*50 + Style.RESET_ALL)
        print(f"\nAdivinhe o número entre 1 e 100")
        print(f"Você tem {self.max_tentativas} tentativas\n")
        
        while self.tentativas < self.max_tentativas:
            try:
                palpite = int(input(f"Tentativa {self.tentativas + 1}: "))
                self.tentativas += 1
                self.historico.append(palpite)
                
                if palpite == self.numero_secreto:
                    print(Fore.GREEN + f"\n🎉 PARABÉNS! Você acertou em {self.tentativas} tentativas!")
                    self.mostrar_historico()
                    return
                
                diferenca = abs(palpite - self.numero_secreto)
                if diferenca <= 5:
                    dica = "MUITO QUENTE! 🔥"
                    cor = Fore.RED
                elif diferenca <= 15:
                    dica = "QUENTE 🔥"
                    cor = Fore.YELLOW
                elif diferenca <= 30:
                    dica = "MORNO 🌡️"
                    cor = Fore.WHITE
                else:
                    dica = "FRIO ❄️"
                    cor = Fore.CYAN
                
                print(cor + f"{dica} - O número é {'maior' if palpite < self.numero_secreto else 'menor'}")
                
            except ValueError:
                print(Fore.RED + "Digite um número válido!")
        
        print(Fore.RED + f"\n💀 Game Over! O número era {self.numero_secreto}")
        self.mostrar_historico()
    
    def mostrar_historico(self):
        print(Fore.YELLOW + "\nSeus palpites:", self.historico)
```

### 10.2 Jogo de Cartas (21/Blackjack simplificado)

```python
import random
import time

class Baralho:
    naipes = ['♥', '♦', '♣', '♠']
    valores = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']
    
    def __init__(self):
        self.cartas = []
        self.embaralhar()
    
    def criar_baralho(self):
        for naipe in self.naipes:
            for valor in self.valores:
                self.cartas.append((valor, naipe))
    
    def embaralhar(self):
        self.cartas = []
        self.criar_baralho()
        random.shuffle(self.cartas)
    
    def comprar(self):
        return self.cartas.pop()

class Jogo21:
    def __init__(self):
        self.baralho = Baralho()
        self.jogador = []
        self.dealer = []
        self.jogador_parou = False
    
    def valor_mao(self, mao):
        valor = 0
        ases = 0
        for carta in mao:
            if carta[0] in ['J', 'Q', 'K']:
                valor += 10
            elif carta[0] == 'A':
                ases += 1
                valor += 11
            else:
                valor += int(carta[0])
        
        # Ajusta ases se estourar
        while valor > 21 and ases > 0:
            valor -= 10
            ases -= 1
        
        return valor
    
    def mostrar_mao(self, mao, jogador="Jogador"):
        print(f"\n{jogador}: ", end="")
        for carta in mao:
            if carta[1] in ['♥', '♦']:
                cor = Fore.RED
            else:
                cor = Fore.WHITE
            print(cor + f"[{carta[0]}{carta[1]}]" + Style.RESET_ALL, end=" ")
        print(f" (Total: {self.valor_mao(mao)})")
    
    def jogar(self):
        print(Fore.CYAN + "\n" + "="*50)
        print(centralizar("BLACKJACK", 50))
        print("="*50 + Style.RESET_ALL)
        
        # Distribui cartas iniciais
        self.jogador.append(self.baralho.comprar())
        self.dealer.append(self.baralho.comprar())
        self.jogador.append(self.baralho.comprar())
        self.dealer.append(self.baralho.comprar())
        
        while not self.jogador_parou:
            self.mostrar_mao(self.jogador, "Jogador")
            self.mostrar_mao([self.dealer[0]], "Dealer (primeira carta)")
            
            valor = self.valor_mao(self.jogador)
            if valor > 21:
                print(Fore.RED + "\n💥 Estourou! Você perdeu!")
                return
            elif valor == 21:
                print(Fore.GREEN + "\n🎉 BLACKJACK!")
                break
            
            acao = input("\nQuer mais uma carta? (s/n): ").lower()
            if acao == 's':
                self.jogador.append(self.baralho.comprar())
            else:
                self.jogador_parou = True
        
        # Vez do dealer
        print("\n" + "-"*30)
        print("Vez do dealer...")
        time.sleep(1)
        
        self.mostrar_mao(self.dealer, "Dealer")
        
        while self.valor_mao(self.dealer) < 17:
            print("Dealer comprou mais uma carta...")
            time.sleep(1)
            self.dealer.append(self.baralho.comprar())
            self.mostrar_mao(self.dealer, "Dealer")
        
        valor_jogador = self.valor_mao(self.jogador)
        valor_dealer = self.valor_mao(self.dealer)
        
        print("\n" + "="*30)
        if valor_dealer > 21:
            print(Fore.GREEN + "🎉 Dealer estourou! Você ganhou!")
        elif valor_jogador > valor_dealer:
            print(Fore.GREEN + "🎉 Você ganhou!")
        elif valor_jogador < valor_dealer:
            print(Fore.RED + "💀 Dealer ganhou!")
        else:
            print(Fore.YELLOW + "🤝 Empate!")
```

---

## 11. Dicas e Boas Práticas {#dicas}

### 11.1 Organização do Código

```python
# Boa prática: separar em módulos
# main.py
from game import Game
from config import configurar_terminal

def main():
    configurar_terminal()
    jogo = Game()
    jogo.iniciar()

if __name__ == "__main__":
    main()
```

### 11.2 Salvamento e Carregamento

```python
import json
import os

class SaveManager:
    @staticmethod
    def salvar_jogo(jogador, arquivo="save.json"):
        dados = {
            "nome": jogador.nome,
            "nivel": jogador.nivel,
            "vida": jogador.vida,
            "vida_max": jogador.vida_max,
            "xp": jogador.xp,
            "ouro": jogador.ouro,
            "inventario": [item.nome for item in jogador.inventario]
        }
        
        with open(arquivo, 'w', encoding='utf-8') as f:
            json.dump(dados, f, indent=4, ensure_ascii=False)
        print("Jogo salvo!")
    
    @staticmethod
    def carregar_jogo(arquivo="save.json"):
        if not os.path.exists(arquivo):
            return None
        
        with open(arquivo, 'r', encoding='utf-8') as f:
            dados = json.load(f)
        
        jogador = Jogador(dados["nome"])
        jogador.nivel = dados["nivel"]
        jogador.vida = dados["vida"]
        jogador.vida_max = dados["vida_max"]
        jogador.xp = dados["xp"]
        jogador.ouro = dados["ouro"]
        
        return jogador
```

### 11.3 Sistema de Log e Debug

```python
class Logger:
    DEBUG = 0
    INFO = 1
    WARNING = 2
    ERROR = 3
    
    def __init__(self, nivel=INFO):
        self.nivel = nivel
        self.logs = []
    
    def log(self, nivel, mensagem):
        if nivel >= self.nivel:
            timestamp = time.strftime("%H:%M:%S")
            niveis = ["DEBUG", "INFO", "WARNING", "ERROR"]
            prefixo = niveis[nivel]
            
            linha = f"[{timestamp}] {prefixo}: {mensagem}"
            self.logs.append(linha)
            
            if nivel >= self.WARNING:
                print(Fore.YELLOW + linha + Style.RESET_ALL)
            elif nivel >= self.ERROR:
                print(Fore.RED + linha + Style.RESET_ALL)
            elif self.nivel == self.DEBUG:
                print(linha)
    
    def salvar_logs(self, arquivo="game.log"):
        with open(arquivo, 'w', encoding='utf-8') as f:
            f.write('\n'.join(self.logs))
```

### 11.4 Checklist de Desenvolvimento

- [ ] Definir conceito e mecânicas do jogo
- [ ] Estruturar dados do jogo (classes, estados)
- [ ] Implementar entrada do usuário
- [ ] Criar sistema de exibição (cores, formatação)
- [ ] Desenvolver game loop principal
- [ ] Adicionar sistema de salvamento
- [ ] Testar em diferentes terminais
- [ ] Documentar controles e regras
- [ ] Otimizar performance (evitar redraw desnecessário)
- [ ] Adicionar tratamentos de erro

### 11.5 Resumo de Comandos Úteis

```python
# Limpar tela
os.system('cls' if os.name == 'nt' else 'clear')

# Esperar tecla
input("Pressione Enter para continuar...")

# Pausar
time.sleep(segundos)

# Cores (Colorama)
Fore.RED, Fore.GREEN, Fore.BLUE, Fore.YELLOW
Back.RED, Back.GREEN, Back.BLUE
Style.BRIGHT, Style.DIM, Style.RESET_ALL

# Caractere único (cross-platform)
getch()  # Função definida anteriormente

# Redimensionar terminal (Unix)
sys.stdout.write("\x1b[8;24;80t")  # 24 linhas, 80 colunas
```
