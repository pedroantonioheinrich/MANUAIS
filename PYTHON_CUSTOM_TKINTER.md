# Manual Completo de CustomTkinter para Python

CustomTkinter é uma biblioteca moderna que estende o Tkinter padrão, oferecendo widgets com aparência customizável e temas modernos, ideal para criar interfaces gráficas atraentes com facilidade. Este manual cobre desde a instalação até exemplos práticos, explicando cada parte do código e os principais comandos.

## Índice
1. [Introdução](#introdução)
2. [Instalação](#instalação)
3. [Primeiros Passos](#primeiros-passos)
4. [Widgets Principais](#widgets-principais)
   - [CTk](#ctk)
   - [CTkButton](#ctkbutton)
   - [CTkLabel](#ctklabel)
   - [CTkEntry](#ctkentry)
   - [CTkCheckBox](#ctkcheckbox)
   - [CTkRadioButton](#ctkradiobutton)
   - [CTkSwitch](#ctkswitch)
   - [CTkSlider](#ctkslider)
   - [CTkProgressBar](#ctkprogressbar)
   - [CTkComboBox](#ctkcombobox)
   - [CTkOptionMenu](#ctkoptionmenu)
   - [CTkTextbox](#ctktextbox)
   - [CTkScrollbar](#ctkscrollbar)
   - [CTkCanvas](#ctkcanvas)
   - [CTkImage](#ctkimage)
   - [CTkFrame](#ctkframe)
5. [Personalização de Aparência](#personalização-de-aparência)
   - [Temas](#temas)
   - [Cores e Estilos](#cores-e-estilos)
6. [Eventos e Callbacks](#eventos-e-callbacks)
7. [Gerenciamento de Layout](#gerenciamento-de-layout)
8. [Exemplos Práticos](#exemplos-práticos)
9. [Dicas e Boas Práticas](#dicas-e-boas-práticas)
10. [Referências](#referências)

---

## Introdução

Tkinter é a biblioteca gráfica padrão do Python, mas sua aparência pode parecer datada. CustomTkinter oferece widgets modernos e personalizáveis, mantendo a simplicidade do Tkinter. Ele é baseado no Tkinter, então muitos conceitos são similares, mas com melhorias visuais e funcionais.

## Instalação

Instale via pip:

```bash
pip install customtkinter
```

Certifique-se de ter o Python 3.6 ou superior. Para verificar a instalação:

```python
import customtkinter
print(customtkinter.__version__)
```

## Primeiros Passos

Um exemplo mínimo:

```python
import customtkinter as ctk

# Configurar aparência (opcional)
ctk.set_appearance_mode("dark")  # "dark", "light" ou "system"
ctk.set_default_color_theme("blue")  # "blue", "green", "dark-blue", etc.

# Criar janela principal
janela = ctk.CTk()
janela.title("Minha App CustomTkinter")
janela.geometry("400x300")

# Adicionar um botão
botao = ctk.CTkButton(janela, text="Clique Aqui", command=lambda: print("Olá!"))
botao.pack(pady=20)

# Iniciar loop principal
janela.mainloop()
```

**Explicação linha a linha:**

- `import customtkinter as ctk`: importa a biblioteca com alias.
- `ctk.set_appearance_mode(...)`: define o tema claro/escuro.
- `ctk.set_default_color_theme(...)`: define a cor de destaque dos widgets.
- `janela = ctk.CTk()`: cria a janela principal (substitui `tk.Tk()`).
- `janela.title(...)`: define o título da janela.
- `janela.geometry(...)`: define tamanho inicial.
- `ctk.CTkButton(...)`: cria um botão customizado.
- `botao.pack(...)`: posiciona o botão com espaçamento vertical.
- `janela.mainloop()`: inicia o loop de eventos.

## Widgets Principais

Abaixo, detalhamos os principais widgets do CustomTkinter, com seus parâmetros mais comuns e exemplos.

### CTk

A janela principal. Herda de `tk.Tk`, mas com melhorias.

**Métodos importantes:**
- `.geometry("larguraxaltura")`: define tamanho.
- `.title("título")`: define título.
- `.resizable(True, True)`: permite redimensionamento.
- `.iconbitmap("caminho.ico")`: define ícone.
- `.configure(fg_color="cor")`: altera cor de fundo.
- `.destroy()`: fecha a janela.

**Exemplo:**

```python
janela = ctk.CTk()
janela.configure(fg_color="#2b2b2b")
```

### CTkButton

Botão estilizado.

**Parâmetros comuns:**
- `master`: widget pai (janela ou frame).
- `text`: texto do botão.
- `command`: função a ser chamada ao clicar.
- `fg_color`: cor de fundo do botão.
- `hover_color`: cor ao passar o mouse.
- `text_color`: cor do texto.
- `font`: tupla (fonte, tamanho) ex: ("Arial", 14).
- `corner_radius`: raio das bordas arredondadas.
- `width`, `height`: dimensões.
- `state`: "normal", "disabled" ou "active".
- `image`: imagem (objeto CTkImage) ao lado do texto.

**Exemplo:**

```python
def ao_clicar():
    print("Botão clicado!")

botao = ctk.CTkButton(
    master=janela,
    text="Salvar",
    command=ao_clicar,
    fg_color="green",
    hover_color="darkgreen",
    text_color="white",
    corner_radius=10
)
botao.pack()
```

### CTkLabel

Rótulo para exibir texto ou imagem.

**Parâmetros:**
- `text`: texto exibido.
- `image`: imagem (CTkImage) para exibir.
- `compound`: posição da imagem em relação ao texto ("top", "left", "right", "bottom").
- `fg_color`: cor de fundo (pode ser "transparent").
- `text_color`: cor do texto.
- `font`: fonte.
- `justify`: alinhamento do texto ("left", "center", "right").
- `wraplength`: quebra de linha em pixels.

**Exemplo:**

```python
label = ctk.CTkLabel(janela, text="Bem-vindo!", font=("Arial", 20, "bold"))
label.pack()
```

### CTkEntry

Campo de entrada de texto.

**Parâmetros:**
- `placeholder_text`: texto de dica quando vazio.
- `show`: caractere de máscara (ex: "*" para senha).
- `width`, `height`.
- `fg_color`, `text_color`, `border_color`.
- `corner_radius`.
- `state`: "normal" ou "disabled".
- `validate`: comando de validação.

**Métodos:**
- `.get()`: obtém o texto.
- `.insert(index, texto)`: insere texto.
- `.delete(início, fim)`: apaga trecho.
- `.bind(evento, função)`: vincula eventos.

**Exemplo:**

```python
entry = ctk.CTkEntry(janela, placeholder_text="Digite seu nome", width=200)
entry.pack()
texto = entry.get()
```

### CTkCheckBox

Caixa de seleção.

**Parâmetros:**
- `text`: rótulo.
- `variable`: variável IntVar ou StringVar para armazenar estado.
- `onvalue`, `offvalue`: valores quando marcado/desmarcado.
- `command`: função chamada ao mudar.
- `checkbox_width`, `checkbox_height`: tamanho do quadrado.
- `fg_color`, `hover_color`, `border_color`.

**Exemplo:**

```python
var_check = ctk.IntVar(value=0)
checkbox = ctk.CTkCheckBox(janela, text="Aceito os termos", variable=var_check)
checkbox.pack()
print(var_check.get())  # 1 se marcado, 0 se não
```

### CTkRadioButton

Botão de opção (seleção única em grupo).

**Parâmetros:**
- `text`: rótulo.
- `variable`: variável compartilhada (IntVar ou StringVar) para o grupo.
- `value`: valor associado a este botão.
- `command`: função ao mudar.

**Exemplo:**

```python
var_radio = ctk.StringVar(value="opcao1")
radio1 = ctk.CTkRadioButton(janela, text="Opção 1", variable=var_radio, value="opcao1")
radio2 = ctk.CTkRadioButton(janela, text="Opção 2", variable=var_radio, value="opcao2")
radio1.pack()
radio2.pack()
```

### CTkSwitch

Interruptor (ligado/desligado).

**Parâmetros:**
- `text`: rótulo.
- `variable`: variável (IntVar ou BooleanVar).
- `onvalue`, `offvalue`.
- `command`: função ao alternar.
- `switch_width`, `switch_height`.
- `fg_color`, `progress_color` (cor quando ligado).

**Exemplo:**

```python
var_switch = ctk.BooleanVar()
switch = ctk.CTkSwitch(janela, text="Modo noturno", variable=var_switch)
switch.pack()
```

### CTkSlider

Controle deslizante.

**Parâmetros:**
- `from_`, `to`: intervalo de valores.
- `number_of_steps`: número de passos discretos (opcional).
- `variable`: variável DoubleVar para vincular.
- `command`: função chamada ao mover.
- `orientation`: "horizontal" (padrão) ou "vertical".
- `fg_color`, `progress_color`, `button_color`.

**Métodos:**
- `.get()`: valor atual.
- `.set(valor)`: define valor.

**Exemplo:**

```python
slider = ctk.CTkSlider(janela, from_=0, to=100, command=lambda v: print(v))
slider.pack()
```

### CTkProgressBar

Barra de progresso.

**Parâmetros:**
- `orientation`: "horizontal" ou "vertical".
- `mode`: "determinate" (valor definido) ou "indeterminate" (animação).
- `variable`: DoubleVar para vincular (0.0 a 1.0).
- `width`, `height`.
- `fg_color`, `progress_color`.
- `border_width`, `corner_radius`.

**Métodos:**
- `.start()`: inicia animação (modo indeterminate).
- `.stop()`: para animação.
- `.set(valor)`: define progresso (0.0 a 1.0).

**Exemplo:**

```python
progress = ctk.CTkProgressBar(janela, mode="determinate")
progress.pack()
progress.set(0.5)  # 50%
```

### CTkComboBox

Caixa de combinação (dropdown com edição opcional).

**Parâmetros:**
- `values`: lista de opções.
- `variable`: StringVar para armazenar seleção.
- `command`: função ao selecionar.
- `state`: "readonly" (apenas seleção) ou "normal" (permite digitar).
- `dropdown_fg_color`, `dropdown_text_color`, `dropdown_hover_color`.

**Métodos:**
- `.get()`: valor atual.
- `.set(valor)`: define valor.

**Exemplo:**

```python
combobox = ctk.CTkComboBox(janela, values=["Opção A", "Opção B", "Opção C"], state="readonly")
combobox.pack()
combobox.set("Opção A")
```

### CTkOptionMenu

Menu de opções (similar ao Combobox, mas sem edição, apenas seleção).

**Parâmetros:**
- `values`: lista.
- `variable`: StringVar.
- `command`: função ao selecionar.
- `fg_color`, `button_color`, `dropdown_color`.

**Exemplo:**

```python
def opcao_selecionada(escolha):
    print(f"Escolheu: {escolha}")

optionmenu = ctk.CTkOptionMenu(janela, values=["Python", "Java", "C++"], command=opcao_selecionada)
optionmenu.pack()
```

### CTkTextbox

Área de texto multilinha.

**Parâmetros:**
- `width`, `height`.
- `fg_color`, `text_color`, `border_color`.
- `font`.
- `wrap`: "char", "word" ou "none".
- `state`: "normal" ou "disabled".
- `scrollbar_button_color`, etc.

**Métodos:**
- `.insert(index, texto)`: insere texto.
- `.get(início, fim)`: obtém texto.
- `.delete(início, fim)`: apaga.
- `.see(index)`: rola para posição.
- `.bind("<Key>", função)`: eventos de tecla.

**Exemplo:**

```python
textbox = ctk.CTkTextbox(janela, width=300, height=200)
textbox.pack()
textbox.insert("0.0", "Texto inicial\n")
```

### CTkScrollbar

Barra de rolagem (geralmente associada a um Textbox ou Canvas).

**Parâmetros:**
- `orientation`: "vertical" ou "horizontal".
- `command`: função a ser chamada ao rolar (geralmente o método `yview` ou `xview` do widget).
- `fg_color`, `button_color`, `button_hover_color`.

**Exemplo com Textbox:**

```python
textbox = ctk.CTkTextbox(janela)
textbox.pack(side="left", fill="both", expand=True)

scrollbar = ctk.CTkScrollbar(janela, orientation="vertical", command=textbox.yview)
scrollbar.pack(side="right", fill="y")

textbox.configure(yscrollcommand=scrollbar.set)
```

### CTkCanvas

Área de desenho (herda de tk.Canvas, mas com suporte a cores do tema).

**Parâmetros:**
- `width`, `height`.
- `bg` (ou `background`): cor de fundo.
- `highlightthickness`: espessura do foco.

**Métodos do Tkinter Canvas:** `create_line`, `create_rectangle`, `create_oval`, etc.

**Exemplo:**

```python
canvas = ctk.CTkCanvas(janela, width=200, height=200, bg="white")
canvas.pack()
canvas.create_rectangle(50, 50, 150, 150, fill="blue")
```

### CTkImage

Manipulação de imagens para uso em widgets.

**Como criar:**
```python
from PIL import Image

img = ctk.CTkImage(
    light_image=Image.open("imagem_clara.png"),   # para tema claro
    dark_image=Image.open("imagem_escura.png"),   # para tema escuro (opcional)
    size=(50, 50)
)
```

Depois usar em botões, labels, etc.

**Exemplo:**

```python
botao = ctk.CTkButton(janela, text="Com imagem", image=img, compound="left")
```

### CTkFrame

Container para agrupar widgets. Herda de `tk.Frame`, mas com estilo customizável.

**Parâmetros:**
- `fg_color`: cor de fundo.
- `border_width`, `border_color`.
- `corner_radius`.
- `width`, `height`.

**Exemplo:**

```python
frame = ctk.CTkFrame(janela, fg_color="gray20", corner_radius=10)
frame.pack(padx=10, pady=10, fill="both", expand=True)
label = ctk.CTkLabel(frame, text="Dentro do frame")
label.pack()
```

## Personalização de Aparência

### Temas

CustomTkinter oferece três modos de aparência:
- `"dark"`: tema escuro
- `"light"`: tema claro
- `"system"`: segue o tema do sistema operacional (se suportado)

Configure com:
```python
ctk.set_appearance_mode("dark")
```

### Cores e Estilos

É possível definir temas de cores personalizados. O padrão inclui:
- `"blue"` (azul)
- `"green"` (verde)
- `"dark-blue"` (azul escuro)

Use:
```python
ctk.set_default_color_theme("green")
```

Para criar um tema personalizado, crie um arquivo `.json` com as cores e carregue:
```python
ctk.set_default_color_theme("caminho/meu_tema.json")
```

Exemplo de arquivo de tema (`meu_tema.json`):
```json
{
    "CTk": {
        "fg_color": ["#2b2b2b", "#dbdbdb"]
    },
    "CTkButton": {
        "fg_color": ["#3a7ebf", "#1f538d"],
        "hover_color": ["#325882", "#14375e"],
        "text_color": ["#ffffff", "#ffffff"]
    }
}
```

Cada cor pode ser uma lista com dois elementos: [cor para tema escuro, cor para tema claro].

### Personalizando Widgets Individualmente

Todos os widgets aceitam parâmetros de cor diretamente:
- `fg_color`: cor de fundo.
- `text_color`: cor do texto.
- `hover_color`: cor ao passar o mouse (botões, etc).
- `border_color`: cor da borda.
- `progress_color`: cor de progresso (slider, progressbar).

Use códigos hexadecimais ou nomes de cores do Tkinter (ex: "red").

## Eventos e Callbacks

CustomTkinter usa o mesmo sistema de eventos do Tkinter. Você pode vincular funções a eventos com `.bind()`.

**Eventos comuns:**
- `<Button-1>`: clique com botão esquerdo
- `<Button-3>`: clique com botão direito
- `<Double-Button-1>`: duplo clique
- `<Enter>`: mouse entra no widget
- `<Leave>`: mouse sai do widget
- `<Key>`: tecla pressionada (requer foco)
- `<FocusIn>`: widget ganha foco
- `<FocusOut>`: perde foco

**Exemplo:**

```python
def ao_entrar(evento):
    print("Mouse entrou no botão")

botao.bind("<Enter>", ao_entrar)
```

Para eventos de teclado, é necessário que o widget tenha foco. Use `focus_set()` para dar foco.

## Gerenciamento de Layout

CustomTkinter utiliza os mesmos gerenciadores de layout do Tkinter: pack, grid e place.

### pack
Organiza os widgets em blocos.
- `side`: "top", "bottom", "left", "right"
- `fill`: "x", "y", "both" (preenche espaço)
- `expand`: True/False (expande com o container)
- `padx`, `pady`: espaçamento externo
- `ipadx`, `ipady`: espaçamento interno

**Exemplo:**
```python
botao.pack(side="top", fill="x", padx=10, pady=5)
```

### grid
Organiza em linhas e colunas.
- `row`, `column`
- `rowspan`, `columnspan`
- `sticky`: "n", "s", "e", "w" (combinações) para alinhar
- `padx`, `pady`

**Exemplo:**
```python
label.grid(row=0, column=0, padx=5, pady=5)
entry.grid(row=0, column=1, padx=5, pady=5)
```

### place
Posicionamento absoluto.
- `x`, `y`: coordenadas
- `width`, `height`
- `relx`, `rely`: posição relativa (0 a 1)
- `anchor`: ponto de ancoragem ("nw", "center", etc.)

**Exemplo:**
```python
botao.place(relx=0.5, rely=0.5, anchor="center")
```

## Exemplos Práticos

### Exemplo 1: Calculadora Simples

```python
import customtkinter as ctk

def calcular():
    try:
        num1 = float(entry1.get())
        num2 = float(entry2.get())
        op = operacao.get()
        if op == "+":
            resultado = num1 + num2
        elif op == "-":
            resultado = num1 - num2
        elif op == "*":
            resultado = num1 * num2
        elif op == "/":
            resultado = num1 / num2
        label_resultado.configure(text=f"Resultado: {resultado}")
    except:
        label_resultado.configure(text="Erro")

# Configurar janela
ctk.set_appearance_mode("dark")
janela = ctk.CTk()
janela.title("Calculadora")
janela.geometry("300x300")

# Widgets
entry1 = ctk.CTkEntry(janela, placeholder_text="Número 1")
entry1.pack(pady=5)

entry2 = ctk.CTkEntry(janela, placeholder_text="Número 2")
entry2.pack(pady=5)

operacao = ctk.CTkComboBox(janela, values=["+", "-", "*", "/"], state="readonly")
operacao.pack(pady=5)
operacao.set("+")

botao = ctk.CTkButton(janela, text="Calcular", command=calcular)
botao.pack(pady=10)

label_resultado = ctk.CTkLabel(janela, text="Resultado:")
label_resultado.pack(pady=5)

janela.mainloop()
```

### Exemplo 2: Formulário com Abas (CTkTabview)

CustomTkinter não tem um widget de abas nativo, mas podemos usar `CTkTabview` (disponível em versões recentes) ou criar com frames.

```python
import customtkinter as ctk

janela = ctk.CTk()
janela.geometry("500x400")

tabview = ctk.CTkTabview(janela, width=450, height=350)
tabview.pack(padx=20, pady=20)

# Criar abas
tab1 = tabview.add("Dados Pessoais")
tab2 = tabview.add("Preferências")

# Conteúdo da aba 1
ctk.CTkLabel(tab1, text="Nome:").grid(row=0, column=0, padx=10, pady=10)
entry_nome = ctk.CTkEntry(tab1)
entry_nome.grid(row=0, column=1, padx=10, pady=10)

ctk.CTkLabel(tab1, text="Email:").grid(row=1, column=0, padx=10, pady=10)
entry_email = ctk.CTkEntry(tab1)
entry_email.grid(row=1, column=1, padx=10, pady=10)

# Conteúdo da aba 2
var_notificacoes = ctk.BooleanVar()
checkbox_notif = ctk.CTkCheckBox(tab2, text="Receber notificações", variable=var_notificacoes)
checkbox_notif.pack(pady=20)

slider = ctk.CTkSlider(tab2, from_=0, to=100)
slider.pack(pady=10)
slider.set(50)

janela.mainloop()
```

### Exemplo 3: Barra de Progresso com Thread

Para não travar a interface, use threads para tarefas longas.

```python
import customtkinter as ctk
import time
import threading

def tarefa_longa():
    for i in range(101):
        time.sleep(0.05)
        progress.set(i/100)
        percentual.configure(text=f"{i}%")
    botao.configure(state="normal")

def iniciar():
    botao.configure(state="disabled")
    thread = threading.Thread(target=tarefa_longa)
    thread.daemon = True
    thread.start()

janela = ctk.CTk()
janela.geometry("300x200")

progress = ctk.CTkProgressBar(janela, mode="determinate")
progress.pack(pady=20)
progress.set(0)

percentual = ctk.CTkLabel(janela, text="0%")
percentual.pack()

botao = ctk.CTkButton(janela, text="Iniciar", command=iniciar)
botao.pack(pady=20)

janela.mainloop()
```

## Dicas e Boas Práticas

1. **Organize o código**: use classes para separar a lógica da interface.
2. **Evite bloquear a mainloop**: tarefas demoradas devem rodar em threads ou usar `after`.
3. **Use `after` para agendamentos**:
   ```python
   def atualizar():
       # código
       janela.after(1000, atualizar)  # chama a cada 1 segundo
   atualizar()
   ```
4. **Varíaveis de controle**: use `StringVar`, `IntVar`, etc., para sincronizar dados entre widgets.
5. **Destrua objetos corretamente**: ao fechar, use `janela.destroy()`.
6. **Explore a documentação oficial**: [CustomTkinter Documentation](https://customtkinter.tomschimansky.com/).

## Referências

- [Documentação oficial do CustomTkinter](https://customtkinter.tomschimansky.com/)
- [Repositório GitHub](https://github.com/TomSchimansky/CustomTkinter)
- [Tkinter documentation (para referência de eventos e métodos base)](https://docs.python.org/3/library/tkinter.html)

---
