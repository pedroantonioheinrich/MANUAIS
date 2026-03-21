# Manual Focado em Criação e Gerenciamento de Janelas com CustomTkinter

Este manual detalha todas as funcionalidades e comandos relacionados à criação, configuração e manutenção de janelas utilizando a biblioteca CustomTkinter. O foco é fornecer uma compreensão profunda de cada aspecto das janelas, desde a janela principal até janelas secundárias e diálogos modais.

---

## 1. Introdução

CustomTkinter é uma extensão moderna do Tkinter que oferece widgets com aparência personalizável e suporte nativo a temas claro/escuro. A classe central para janelas é `CTk` (subclasse de `tkinter.Tk`), que representa a janela principal da aplicação. Janelas adicionais são criadas com `CTkToplevel`. Este manual explora cada comando e parâmetro relacionado a essas janelas.

---

## 2. Instalação e Importação

Instale a biblioteca com pip:

```bash
pip install customtkinter
```

No código, importe o módulo (geralmente abreviado como `ctk`):

```python
import customtkinter as ctk
```

---

## 3. A Janela Principal (CTk)

A classe `CTk` é a base de qualquer aplicação. Ela herda de `tkinter.Tk` e adiciona recursos visuais modernos.

### 3.1. Criando a Janela Principal

```python
import customtkinter as ctk

# Configuração global (opcional, mas recomendada antes da criação da janela)
ctk.set_appearance_mode("dark")      # "dark", "light", "system"
ctk.set_default_color_theme("blue")  # "blue", "green", "dark-blue"

# Instancia a janela principal
janela = ctk.CTk()
```

**Explicação:**
- `ctk.set_appearance_mode()` define o modo de aparência para todas as janelas criadas depois. Valores: `"dark"`, `"light"`, `"system"` (segue o sistema operacional).
- `ctk.set_default_color_theme()` define o tema de cores padrão. Pode ser um dos temas embutidos ou um caminho para um arquivo de tema personalizado.

### 3.2. Configurações Básicas da Janela

Após criar a instância, você pode configurar diversos atributos da janela:

| Método/Atributo | Descrição | Exemplo |
|----------------|-----------|---------|
| `title(string)` | Define o título da janela | `janela.title("Meu App")` |
| `geometry(string)` | Define tamanho e posição inicial: `"larguraxaltura+x+y"` | `janela.geometry("800x600")` ou `"800x600+100+50"` |
| `resizable(width, height)` | Permite redimensionamento (True/False) | `janela.resizable(False, False)` |
| `iconbitmap(caminho)` | Define o ícone da janela (arquivo .ico) | `janela.iconbitmap("icone.ico")` |
| `iconphoto(imagem)` | Define ícone usando imagem PhotoImage | `img = tk.PhotoImage(file="icone.png")`<br>`janela.iconphoto(True, img)` |
| `minsize(width, height)` | Define tamanho mínimo | `janela.minsize(400, 300)` |
| `maxsize(width, height)` | Define tamanho máximo | `janela.maxsize(1200, 800)` |
| `attributes(attribute, value)` | Atributos específicos do gerenciador de janelas | `janela.attributes("-topmost", True)` (sempre visível) |

**Exemplo completo:**

```python
janela = ctk.CTk()
janela.title("Aplicativo CustomTkinter")
janela.geometry("800x600")
janela.resizable(True, True)
janela.minsize(400, 300)
janela.iconbitmap("icone.ico")  # se disponível
janela.attributes("-topmost", True)  # janela sempre à frente
```

### 3.3. Métodos de Controle da Janela

| Método | Descrição |
|--------|-----------|
| `mainloop()` | Inicia o loop principal de eventos; a janela é exibida e a aplicação começa a responder. |
| `destroy()` | Destrói a janela e encerra a aplicação (se for a principal). |
| `quit()` | Sai do loop principal, mas pode não destruir a janela. Geralmente usa-se `destroy()`. |
| `withdraw()` | Esconde a janela (sem destruí-la). |
| `deiconify()` | Mostra a janela novamente após `withdraw()`. |
| `iconify()` | Minimiza a janela. |
| `state()` | Retorna o estado atual: `"normal"`, `"iconic"`, `"withdrawn"`. |
| `focus_force()` | Força a janela a receber foco. |

**Exemplo de uso:**

```python
def minimizar():
    janela.iconify()

def esconder():
    janela.withdraw()

def restaurar():
    janela.deiconify()

botao_min = ctk.CTkButton(janela, text="Minimizar", command=minimizar)
botao_min.pack()
```

### 3.4. Eventos da Janela

O método `protocol()` permite capturar eventos do gerenciador de janelas, como o fechamento (clique no "X").

```python
def ao_fechar():
    print("Janela sendo fechada")
    janela.destroy()  # ou janela.quit()

janela.protocol("WM_DELETE_WINDOW", ao_fechar)
```

Outros eventos podem ser capturados com `bind()`:

```python
# Quando a janela ganha foco
janela.bind("<FocusIn>", lambda e: print("Foco obtido"))

# Quando a janela perde foco
janela.bind("<FocusOut>", lambda e: print("Foco perdido"))

# Quando a janela é redimensionada
janela.bind("<Configure>", lambda e: print(f"Nova geometria: {janela.geometry()}"))
```

---

## 4. Janelas Secundárias (CTkToplevel)

Para criar novas janelas (janelas filhas, diálogos, etc.), use a classe `CTkToplevel`. Ela herda de `tkinter.Toplevel` e possui os mesmos métodos de configuração da janela principal.

### 4.1. Criando uma Janela Secundária

```python
def abrir_janela():
    nova = ctk.CTkToplevel(janela)
    nova.title("Janela Secundária")
    nova.geometry("400x300")
    nova.resizable(False, False)

    label = ctk.CTkLabel(nova, text="Conteúdo da janela")
    label.pack(pady=20)

botao = ctk.CTkButton(janela, text="Abrir", command=abrir_janela)
botao.pack()
```

**Observação:** Ao fechar uma `CTkToplevel`, ela é destruída, mas a aplicação continua executando. O comportamento padrão ao clicar no "X" é destruir a janela.

### 4.2. Personalizando o Fechamento da Janela Secundária

É possível interceptar o fechamento com `protocol`:

```python
def fechar_janela_secundaria(janela_sec):
    # Realiza alguma ação antes de fechar
    print("Fechando janela secundária")
    janela_sec.destroy()

def abrir_janela():
    nova = ctk.CTkToplevel(janela)
    nova.title("Janela Secundária")
    nova.geometry("400x300")
    nova.protocol("WM_DELETE_WINDOW", lambda: fechar_janela_secundaria(nova))
```

### 4.3. Gerenciamento de Múltiplas Janelas

Para evitar a criação de múltiplas instâncias da mesma janela, utilize uma variável para guardar a referência e verifique se ela ainda existe:

```python
class Aplicacao:
    def __init__(self):
        self.janela = ctk.CTk()
        self.janela.title("Principal")
        self.janela.geometry("400x300")

        self.janela_config = None  # referência à janela de configuração

        btn = ctk.CTkButton(self.janela, text="Configurações", command=self.abrir_config)
        btn.pack()

    def abrir_config(self):
        if self.janela_config is None or not self.janela_config.winfo_exists():
            self.janela_config = ctk.CTkToplevel(self.janela)
            self.janela_config.title("Configurações")
            self.janela_config.geometry("300x200")
            self.janela_config.protocol("WM_DELETE_WINDOW", self.fechar_config)
        else:
            self.janela_config.focus()

    def fechar_config(self):
        self.janela_config.destroy()
        self.janela_config = None

    def run(self):
        self.janela.mainloop()

if __name__ == "__main__":
    app = Aplicacao()
    app.run()
```

### 4.4. Janelas Modais (bloqueiam a interação com a janela principal)

Uma janela modal impede que o usuário interaja com a janela principal enquanto estiver aberta. Em Tkinter/CustomTkinter, isso pode ser feito com `grab_set()` e `wait_window()`.

```python
def abrir_modal():
    modal = ctk.CTkToplevel(janela)
    modal.title("Modal")
    modal.geometry("300x200")
    modal.transient(janela)          # define que a janela é temporária em relação à principal
    modal.grab_set()                 # captura todos os eventos
    modal.focus_force()              # foco na janela modal

    label = ctk.CTkLabel(modal, text="Essa é uma janela modal")
    label.pack(pady=20)

    def fechar():
        modal.destroy()
    btn = ctk.CTkButton(modal, text="Fechar", command=fechar)
    btn.pack()

    # Aguarda até que a janela modal seja fechada
    janela.wait_window(modal)
```

**Explicação:**
- `transient(janela_principal)`: indica que a janela modal é temporária e não deve aparecer na barra de tarefas separada.
- `grab_set()`: direciona todos os eventos (cliques, teclas) para a janela modal.
- `wait_window(modal)`: pausa a execução até que a janela modal seja destruída.

---

## 5. Personalização Visual das Janelas

Além das configurações básicas, as janelas em CustomTkinter herdam as configurações de aparência global. Mas também é possível personalizar individualmente.

### 5.1. Modo Claro/Escuro por Janela

Embora `set_appearance_mode` defina o modo global, você pode alterar o modo de uma janela específica após sua criação usando o método `configure` com o atributo `appearance_mode`:

```python
janela_escura = ctk.CTk()
janela_escura.configure(appearance_mode="dark")

janela_clara = ctk.CTk()
janela_clara.configure(appearance_mode="light")
```

**Nota:** Isso funciona para `CTk` e `CTkToplevel`.

### 5.2. Tema de Cores por Janela

Da mesma forma, é possível definir um tema diferente para cada janela:

```python
janela.configure(color_theme="green")
```

### 5.3. Cores de Fundo e Borda

As janelas em CustomTkinter têm uma cor de fundo que pode ser alterada:

```python
janela.configure(fg_color="gray")  # define a cor de fundo
```

Também é possível modificar a cor da borda (se houver) e outras propriedades visuais, mas essas são geralmente controladas pelo tema.

### 5.4. Removendo a Barra de Título (janela sem bordas)

Para criar uma janela sem barra de título (útil para splash screens ou janelas personalizadas), use o atributo `"-fullscreen"` ou `"-overrideredirect"`:

```python
janela = ctk.CTk()
janela.overrideredirect(True)  # remove bordas e barra de título
janela.geometry("300x200")
janela.attributes("-topmost", True)  # mantém sempre à frente

# Adicione um botão para fechar
btn_fechar = ctk.CTkButton(janela, text="Fechar", command=janela.destroy)
btn_fechar.pack()
```

**Cuidado:** Com `overrideredirect(True)`, o usuário não pode mover ou redimensionar a janela com o mouse. Você precisará implementar essas funcionalidades manualmente, se necessário.

---

## 6. Posicionamento e Layout Interno

Embora este manual foque nas janelas, o conteúdo dentro delas é organizado com os gerenciadores de geometria `pack`, `grid` e `place`. Estes são aplicados aos frames e widgets filhos.

### 6.1. Utilizando Frames para Organização

A prática comum é criar um `CTkFrame` principal dentro da janela para facilitar o layout:

```python
frame_principal = ctk.CTkFrame(janela)
frame_principal.pack(fill="both", expand=True, padx=20, pady=20)
# Agora coloque os widgets dentro do frame_principal
```

### 6.2. Centralizando a Janela na Tela

Para centralizar a janela na tela, use o método `geometry` calculando as dimensões da tela:

```python
def centralizar_janela(janela, largura, altura):
    screen_width = janela.winfo_screenwidth()
    screen_height = janela.winfo_screenheight()
    x = (screen_width // 2) - (largura // 2)
    y = (screen_height // 2) - (altura // 2)
    janela.geometry(f"{largura}x{altura}+{x}+{y}")

# Exemplo de uso:
centralizar_janela(janela, 800, 600)
```

---

## 7. Comunicação entre Janelas

Para compartilhar dados entre a janela principal e janelas secundárias, utilize variáveis, funções de callback ou passe a referência da janela principal.

### 7.1. Usando Variáveis Compartilhadas

```python
def abrir_config():
    config = ctk.CTkToplevel(janela)
    config.title("Configurações")

    # Variável compartilhada
    var_nome = ctk.StringVar(value=label_nome.cget("text"))

    entry = ctk.CTkEntry(config, textvariable=var_nome)
    entry.pack(pady=10)

    def salvar():
        label_nome.configure(text=var_nome.get())
        config.destroy()

    btn = ctk.CTkButton(config, text="Salvar", command=salvar)
    btn.pack()

# Na janela principal:
label_nome = ctk.CTkLabel(janela, text="Usuário")
label_nome.pack()
```

### 7.2. Usando Callbacks (Funções de Retorno)

Passe uma função da janela principal para a secundária, que será chamada quando necessário.

```python
def atualizar_texto(novo_texto):
    label_nome.configure(text=novo_texto)

def abrir_config(callback):
    config = ctk.CTkToplevel(janela)
    entry = ctk.CTkEntry(config)
    entry.pack()
    btn = ctk.CTkButton(config, text="OK", command=lambda: callback(entry.get()))
    btn.pack()

# Chamada:
abrir_config(atualizar_texto)
```

---

## 8. Exemplos Práticos

### 8.1. Exemplo 1: Janela Principal com Barra de Menu Personalizada

Embora CustomTkinter não tenha um widget de menu nativo, você pode criar um menu horizontal com botões.

```python
class App(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Menu Personalizado")
        self.geometry("600x400")

        # Frame do menu
        menu_frame = ctk.CTkFrame(self, height=50, corner_radius=0)
        menu_frame.pack(fill="x", side="top")

        # Botões do menu
        btn_inicio = ctk.CTkButton(menu_frame, text="Início", command=self.mostrar_inicio)
        btn_inicio.pack(side="left", padx=10)

        btn_config = ctk.CTkButton(menu_frame, text="Configurações", command=self.mostrar_config)
        btn_config.pack(side="left", padx=10)

        # Frame de conteúdo
        self.conteudo = ctk.CTkFrame(self)
        self.conteudo.pack(fill="both", expand=True, padx=20, pady=20)

        self.mostrar_inicio()

    def mostrar_inicio(self):
        self.limpar_conteudo()
        label = ctk.CTkLabel(self.conteudo, text="Página Inicial", font=("Arial", 24))
        label.pack(pady=20)

    def mostrar_config(self):
        self.limpar_conteudo()
        # Exemplo: abrir janela de configurações secundária
        self.config_janela = ctk.CTkToplevel(self)
        self.config_janela.title("Configurações")
        self.config_janela.geometry("400x300")
        # ... adicione widgets de configuração

    def limpar_conteudo(self):
        for widget in self.conteudo.winfo_children():
            widget.destroy()

if __name__ == "__main__":
    app = App()
    app.mainloop()
```

### 8.2. Exemplo 2: Janela de Confirmação Modal

```python
def mostrar_confirmacao(mensagem, callback_confirmar):
    modal = ctk.CTkToplevel(janela)
    modal.title("Confirmação")
    modal.geometry("300x150")
    modal.transient(janela)
    modal.grab_set()
    modal.focus_force()

    label = ctk.CTkLabel(modal, text=mensagem)
    label.pack(pady=10)

    def on_confirmar():
        modal.destroy()
        callback_confirmar()

    def on_cancelar():
        modal.destroy()

    btn_frame = ctk.CTkFrame(modal)
    btn_frame.pack(pady=10)
    btn_ok = ctk.CTkButton(btn_frame, text="Confirmar", command=on_confirmar)
    btn_ok.pack(side="left", padx=10)
    btn_cancel = ctk.CTkButton(btn_frame, text="Cancelar", command=on_cancelar)
    btn_cancel.pack(side="left", padx=10)

# Uso:
def executar_acao():
    print("Ação confirmada!")

botao = ctk.CTkButton(janela, text="Excluir", command=lambda: mostrar_confirmacao("Deseja excluir?", executar_acao))
botao.pack()
```

---

## 9. Boas Práticas

1. **Organização em classes:** Estruture cada janela como uma classe para facilitar manutenção e reaproveitamento.
2. **Evite variáveis globais:** Use atributos de instância ou passe dados via parâmetros.
3. **Gerencie referências:** Mantenha referências a janelas secundárias para evitar múltiplas instâncias e garantir que sejam destruídas corretamente.
4. **Trate fechamentos:** Sempre defina `protocol("WM_DELETE_WINDOW")` quando precisar executar ações antes de fechar uma janela.
5. **Utilize `after` para atualizações periódicas:** Evite loops de espera que bloqueiam a interface. Use `janela.after(tempo, funcao)` para agendar tarefas.
6. **Documente:** Comente o propósito de cada janela e seus métodos.

---
