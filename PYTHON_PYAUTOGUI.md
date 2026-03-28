PyAutoGUI é uma biblioteca Python incrível para automatizar tarefas repetitivas, controlando o mouse e o teclado como se fosse um usuário real. Ela é uma excelente opção para quem precisa de uma solução rápida e simples, sem as complexidades de outras ferramentas .

Este manual completo aborda desde a instalação até técnicas avançadas, com exemplos práticos para você começar a automatizar hoje mesmo.

---

### 📖 Índice
1.  [Introdução e Instalação](#introdução-e-instalação)
2.  [Configurações Iniciais Essenciais](#configurações-iniciais-essenciais)
    - Pausa entre Comandos (PAUSE)
    - Modo de Segurança (FAILSAFE)
3.  [Controle do Mouse](#controle-do-mouse)
    - Obtendo e Movendo o Mouse
    - Cliques (simples, duplo, direito, arrastar)
    - Rolagem (Scroll)
4.  [Controle do Teclado](#controle-do-teclado)
    - Escrevendo Texto
    - Pressionando Teclas
    - Atalhos (Combinações de Teclas)
5.  [Interação com a Tela](#interação-com-a-tela)
    - Captura de Tela (Screenshot)
    - Reconhecimento de Imagens (locateOnScreen)
    - Identificando Cores (pixel)
6.  [Exemplos Práticos e Aplicações](#exemplos-práticos-e-aplicações)
    - Preenchendo um Formulário Automaticamente
    - Baixando Arquivos em Lote
    - Copiando e Colando entre Janelas
7.  [Dicas, Boas Práticas e Limitações](#dicas-boas-práticas-e-limitações)

---

### 1. Introdução e Instalação

O PyAutoGUI é uma biblioteca Python que permite controlar o mouse e o teclado para automatizar interações com outros aplicativos. Ele funciona no Windows, macOS e Linux .

Para instalar, abra seu terminal (ou prompt de comando) e execute o comando:

```bash
pip install pyautogui
```

Para começar a usar, basta importá-lo em seu script:

```python
import pyautogui
import time # Biblioteca nativa muito útil para pausas
```

### 2. Configurações Iniciais Essenciais

Antes de mais nada, configure estas duas opções para evitar comportamentos inesperados:

- **`pyautogui.PAUSE`**: Define uma pausa padrão (em segundos) entre cada comando. Isso dá tempo para o computador processar cada ação e é uma das práticas mais importantes para garantir a estabilidade da automação .

    ```python
    import pyautogui
    pyautogui.PAUSE = 0.5 # Aguarda 0.5 segundos após cada comando
    ```

- **`pyautogui.FAILSAFE`**: É um recurso de segurança. Se ativado (True), você pode mover o mouse rapidamente para o **canto superior esquerdo da tela** para interromper imediatamente o script. Isso é uma "válvula de escape" que pode salvar sua automação de dar errado .

    ```python
    pyautogui.FAILSAFE = True # Ativado por padrão
    ```

### 3. Controle do Mouse

#### Obtendo e Movendo o Mouse

- **`pyautogui.position()`**: Retorna as coordenadas X e Y atuais do cursor do mouse. Essencial para descobrir onde clicar .

    ```python
    # Dê um tempo para posicionar o mouse e descubra sua posição
    time.sleep(2)
    x, y = pyautogui.position()
    print(f'Posição do mouse: x={x}, y={y}')
    ```

- **`pyautogui.size()`**: Retorna a largura e altura da sua tela .

    ```python
    largura, altura = pyautogui.size()
    print(f'Sua tela tem {largura}x{altura} pixels')
    ```

- **`pyautogui.moveTo(x, y, duration)`**: Move o mouse para as coordenadas (x, y). O parâmetro `duration` (em segundos) suaviza o movimento .

    ```python
    # Move o mouse para (500, 500) ao longo de 1 segundo
    pyautogui.moveTo(500, 500, duration=1)
    ```

#### Cliques

- **`pyautogui.click(x, y, button)`**: Simula um clique. Se `x` e `y` não forem fornecidos, clica na posição atual do mouse. O parâmetro `button` pode ser `'left'` (padrão), `'middle'` ou `'right'` .

    ```python
    pyautogui.click(100, 150)  # Clique esquerdo na posição (100, 150)
    pyautogui.rightClick(200, 250)  # Clique direito
    pyautogui.doubleClick(300, 350) # Clique duplo
    ```

- **`pyautogui.mouseDown()` e `pyautogui.mouseUp()`**: São usados para ações de arrastar (drag and drop), combinadas com `moveTo` .

    ```python
    # Exemplo de arrastar um arquivo
    pyautogui.moveTo(567, 38)   # Vai até a posição do arquivo
    pyautogui.mouseDown()       # Segura o botão do mouse
    pyautogui.moveTo(756, 635)  # Move o mouse para o destino
    pyautogui.mouseUp()         # Solta o botão
    ```

#### Rolagem (Scroll)

- **`pyautogui.scroll(amount)`**: Simula a rolagem do mouse. Valores positivos rolam para cima, negativos para baixo .

    ```python
    pyautogui.scroll(5)  # Rola 5 "unidades" para cima
    pyautogui.scroll(-10) # Rola 10 "unidades" para baixo
    ```

### 4. Controle do Teclado

#### Escrevendo Texto

- **`pyautogui.write(text, interval)`**: Escreve o texto fornecido. O parâmetro `interval` (em segundos) adiciona uma pequena pausa entre cada caractere, simulando uma digitação humana .

    ```python
    pyautogui.write('Olá, mundo!')
    pyautogui.write('Texto mais lento', interval=0.1)
    ```

#### Pressionando Teclas

- **`pyautogui.press(key)`**: Simula pressionar e soltar uma única tecla. Exemplos de teclas: `'enter'`, `'esc'`, `'space'`, `'tab'`, `'win'`, `'ctrl'`, etc. .

    ```python
    pyautogui.press('enter')
    pyautogui.press('win') # Abre o menu iniciar do Windows
    ```

- **`pyautogui.keyDown(key)` e `pyautogui.keyUp(key)`**: Pressiona ou solta uma tecla de forma independente. Útil para ações de segurar uma tecla modificadora .

#### Atalhos (Combinações de Teclas)

- **`pyautogui.hotkey(key1, key2, ...)`**: Pressiona as teclas fornecidas em ordem e as solta na ordem inversa. Perfeito para atalhos como Ctrl+C .

    ```python
    # Exemplos comuns
    pyautogui.hotkey('ctrl', 'c') # Copiar
    pyautogui.hotkey('ctrl', 'v') # Colar
    pyautogui.hotkey('win', 'd')  # Mostrar a área de trabalho
    pyautogui.hotkey('alt', 'tab') # Alternar entre janelas
    ```

### 5. Interação com a Tela

#### Captura de Tela (Screenshot)

- **`pyautogui.screenshot()`**: Captura a tela inteira e retorna um objeto imagem .

    ```python
    # Tira um print da tela e salva como 'meu_print.png'
    screenshot = pyautogui.screenshot()
    screenshot.save('meu_print.png')
    ```

#### Reconhecimento de Imagens

Esta é uma das funcionalidades mais poderosas. Você pode fornecer a imagem de um botão e o PyAutoGUI tenta encontrá-lo na tela.

- **`pyautogui.locateOnScreen('imagem.png')`**: Procura a imagem na tela e retorna um objeto com as coordenadas (esquerda, topo, largura, altura) da primeira ocorrência. Retorna `None` se não encontrar .

- **`pyautogui.locateCenterOnScreen('imagem.png')`**: Semelhante ao anterior, mas retorna diretamente as coordenadas (x, y) do centro da imagem. É o mais prático para clicar em botões .

    ```python
    # Encontra um botão na tela e clica no centro dele
    botao_pos = pyautogui.locateCenterOnScreen('botao_salvar.png')
    if botao_pos:
        pyautogui.click(botao_pos)
    else:
        print('Botão não encontrado na tela!')
    ```

    ⚠️ **Importante**: Para que isso funcione bem, a imagem na tela deve ser **exatamente igual** à que você salvou. É uma boa prática usar imagens pequenas e únicas (como o ícone de um botão) .

#### Identificando Cores

- **`pyautogui.pixel(x, y)`**: Retorna a cor de um pixel específico como uma tupla RGB (Red, Green, Blue). Útil para verificar se algo mudou na tela .

    ```python
    cor = pyautogui.pixel(100, 100)
    print(cor) # Exemplo de saída: (255, 255, 255) que é a cor branca
    ```

### 6. Exemplos Práticos e Aplicações

#### Preenchendo um Formulário Automaticamente

Este script espera alguns segundos, clica em um campo, preenche com dados e envia .

```python
import pyautogui
import time

pyautogui.PAUSE = 0.5

# Passo 1: Dar tempo para o usuário posicionar a janela
time.sleep(3)

# Passo 2: Preencher o primeiro campo
pyautogui.click(x=200, y=300) # Coordenadas do primeiro campo
pyautogui.write('João da Silva')

# Passo 3: Pular para o próximo campo (TAB)
pyautogui.press('tab')
pyautogui.write('joao@email.com')

# Passo 4: Enviar o formulário
pyautogui.hotkey('ctrl', 'enter')
```

#### Baixando Arquivos em Lote

Um exemplo baseado em um projeto real onde se baixava artigos acadêmicos .

```python
import pyautogui
import time
import pandas as pd

pyautogui.PAUSE = 0.5

# Lê a lista de IDs de uma planilha
lista_doi = pd.read_excel('papers_id.xlsx')['DOI']

def abrir_navegador():
    pyautogui.press('win')
    pyautogui.write('chrome')
    pyautogui.press('enter')
    time.sleep(3)

def pesquisar_doi(doi):
    pyautogui.hotkey('ctrl', 'l')  # Foca na barra de endereços
    pyautogui.write('https://sci-hub.in')
    pyautogui.press('enter')
    time.sleep(4)
    pyautogui.write(doi)
    pyautogui.press('enter')
    time.sleep(10)  # Aguarda o PDF carregar

def fazer_download():
    pyautogui.click(1795, 141) # Coordenadas do botão de download
    time.sleep(1)
    pyautogui.press('enter')
    time.sleep(4)

# Inicia a automação
abrir_navegador()
for doi in lista_doi:
    pesquisar_doi(doi)
    fazer_download()
```

#### Copiando e Colando entre Janelas

Este exemplo demonstra como usar `hotkey` para copiar e colar informações entre aplicativos .

```python
import pyautogui
import time

time.sleep(3) # Tempo para selecionar o texto que deseja copiar

# Copia o texto selecionado
pyautogui.hotkey('ctrl', 'c')
time.sleep(0.5)

# Alterna para a próxima janela (Win+4, por exemplo)
pyautogui.hotkey('win', '4')
time.sleep(0.5)

# Cola o texto no novo local
pyautogui.hotkey('ctrl', 'v')
pyautogui.press('enter')
```
