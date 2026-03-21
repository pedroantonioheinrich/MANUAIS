# Manual Detalhado de Uso dos Gerenciadores de Geometria Pack e Grid no CustomTkinter

## 1. Introdução

No desenvolvimento de interfaces gráficas com CustomTkinter (e Tkinter), os **gerenciadores de geometria** são responsáveis por posicionar e dimensionar os widgets dentro da janela ou de um container (como `CTkFrame`). CustomTkinter herda os três gerenciadores clássicos do Tkinter: `pack`, `grid` e `place`. Este manual foca nos dois mais utilizados: **pack** e **grid**, explicando cada comando e suas opções em detalhes.

Compreender esses gerenciadores é fundamental para criar layouts responsivos, organizados e de fácil manutenção.

---

## 2. O Gerenciador `pack`

O `pack` organiza os widgets em **blocos** sequenciais, empilhando-os uns após os outros (como em uma lista). É simples e eficaz para layouts lineares (verticais ou horizontais).

### 2.1. Sintaxe Básica

```python
widget.pack(opcoes...)
```

### 2.2. Opções do `pack`

| Opção        | Descrição                                                                                      | Valores                                                                 |
|--------------|------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `side`       | Lado para o qual o widget será "empurrado". Define a direção do empilhamento.                  | `"top"` (padrão), `"bottom"`, `"left"`, `"right"`                       |
| `fill`       | Define se o widget expande para preencher o espaço extra em uma direção.                        | `"none"` (padrão), `"x"`, `"y"`, `"both"`                               |
| `expand`     | Se `True`, o widget ocupa o espaço extra disponível no container.                               | `True` ou `False` (padrão)                                              |
| `padx`, `pady`| Espaçamento externo (padding) ao redor do widget, em pixels.                                   | inteiro ou tupla (por exemplo, `(5,10)` para esquerda/direita diferentes)|
| `ipadx`, `ipady`| Espaçamento interno (padding) dentro do widget, aumentando seu tamanho.                      | inteiro ou tupla                                                         |
| `anchor`     | Quando o widget não preenche todo o espaço alocado, define onde ele será posicionado.          | `"n"`, `"s"`, `"e"`, `"w"`, `"center"`, `"ne"`, `"nw"`, `"se"`, `"sw"` |
| `before`     | Insere o widget antes (à esquerda ou acima) de outro widget especificado.                       | nome do widget                                                           |
| `after`      | Insere o widget depois (à direita ou abaixo) de outro widget.                                   | nome do widget                                                           |
| `in_`        | Insere o widget dentro de um container específico (útil ao usar `pack` em janelas diferentes). | nome do container                                                        |

### 2.3. Explicação Detalhada de Cada Opção

#### **`side`**
Define a direção do empilhamento. Widgets são adicionados sucessivamente na direção escolhida.

- **`"top"` (padrão):** O widget é colocado no topo do espaço restante, e os próximos widgets são empilhados abaixo.
- **`"bottom"`:** Widget colocado na parte inferior, empilhamento para cima.
- **`"left"`:** Widget colocado à esquerda, empilhamento para a direita.
- **`"right"`:** Widget colocado à direita, empilhamento para a esquerda.

**Exemplo:**

```python
import customtkinter as ctk

janela = ctk.CTk()
janela.geometry("400x300")

# Empilhamento vertical (padrão)
ctk.CTkButton(janela, text="Topo").pack(side="top")
ctk.CTkButton(janela, text="Meio").pack(side="top")
ctk.CTkButton(janela, text="Fim").pack(side="top")   # aparecerá abaixo

# Empilhamento horizontal
ctk.CTkButton(janela, text="Esquerda").pack(side="left")
ctk.CTkButton(janela, text="Direita").pack(side="left")  # ao lado direito do primeiro

janela.mainloop()
```

#### **`fill`**
Controla como o widget ocupa o espaço extra na direção perpendicular ao empilhamento.

- **`"none"` (padrão):** O widget mantém seu tamanho natural (conforme definido por `width`/`height`).
- **`"x"`:** Expande horizontalmente para preencher a largura disponível.
- **`"y"`:** Expande verticalmente para preencher a altura disponível.
- **`"both"`:** Expande em ambas as direções.

**Exemplo:**

```python
frame = ctk.CTkFrame(janela)
frame.pack(fill="both", expand=True)  # frame ocupará toda a área

# Botão que preenche horizontalmente
btn = ctk.CTkButton(frame, text="Preencher X")
btn.pack(fill="x")
```

#### **`expand`**
Quando `True`, o widget recebe espaço extra que sobra no container, distribuído igualmente entre todos os widgets com `expand=True`.

- Se `expand=False`, o widget ocupa apenas o espaço necessário, e o espaço extra é alocado para widgets com `expand=True` ou fica vazio.

**Exemplo:**

```python
# Três botões: o do meio expande
ctk.CTkButton(janela, text="1").pack(side="left")
ctk.CTkButton(janela, text="2 (expande)", width=100).pack(side="left", expand=True)
ctk.CTkButton(janela, text="3").pack(side="left")
```

#### **`padx` e `pady`**
Adicionam espaço externo (margem) ao redor do widget. Podem ser um único número (aplicado a ambos os lados) ou uma tupla `(esquerda/direita, cima/baixo)`.

```python
# Espaçamento de 10px em todos os lados
btn.pack(padx=10, pady=10)

# Espaçamento assimétrico: esquerda 5, direita 10, cima 0, baixo 20
btn.pack(padx=(5,10), pady=(0,20))
```

#### **`ipadx` e `ipady`**
Espaçamento interno: aumentam o tamanho do widget adicionando espaço dentro de suas bordas.

```python
btn.pack(ipadx=20, ipady=10)  # botão fica mais largo e mais alto
```

#### **`anchor`**
Define a posição do widget dentro do espaço que lhe foi alocado quando ele não o preenche completamente. Utiliza pontos cardeais.

**Exemplo:**

```python
# Widget pequeno em um container grande (com expand=True)
frame = ctk.CTkFrame(janela, width=400, height=300)
frame.pack(fill="both", expand=True)

# Botão ancorado no canto superior direito
btn = ctk.CTkButton(frame, text="Ancorado")
btn.pack(anchor="ne")  # northeast
```

#### **`before` e `after`**
Permitem inserir um widget em relação a outro já empacotado. Útil para reorganização dinâmica.

```python
btn1 = ctk.CTkButton(janela, text="Botão 1")
btn1.pack()

btn2 = ctk.CTkButton(janela, text="Botão 2")
btn2.pack(after=btn1)   # coloca btn2 depois (abaixo) de btn1

btn3 = ctk.CTkButton(janela, text="Botão 3")
btn3.pack(before=btn1)  # coloca btn3 antes (acima) de btn1
```

#### **`in_`**
Especifica um container diferente do pai direto para onde o widget será empacotado. O widget deve ser filho desse container ou do pai, mas o gerenciador `pack` agirá no container indicado.

```python
frame1 = ctk.CTkFrame(janela)
frame2 = ctk.CTkFrame(janela)
frame1.pack()
frame2.pack()

# Este botão é filho de frame1, mas será empacotado em frame2
btn = ctk.CTkButton(frame1, text="Especial")
btn.pack(in_=frame2)
```

### 2.4. Ordem de Empacotamento e Espaço Restante

O `pack` aloca espaço sequencialmente. Cada chamada reduz o espaço disponível para os próximos widgets. A opção `expand` faz com que o widget compartilhe o espaço extra restante após todos os widgets com `expand=False` terem sido posicionados.

---

## 3. O Gerenciador `grid`

O `grid` organiza os widgets em uma **grade bidimensional** de linhas e colunas. É ideal para layouts de formulários e painéis complexos.

### 3.1. Sintaxe Básica

```python
widget.grid(opcoes...)
```

### 3.2. Opções do `grid`

| Opção          | Descrição                                                                                     | Valores                                                                 |
|----------------|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `row`          | Índice da linha (0‑based) onde o widget será colocado.                                        | inteiro                                                                 |
| `column`       | Índice da coluna (0‑based) onde o widget será colocado.                                       | inteiro                                                                 |
| `rowspan`      | Número de linhas que o widget ocupará (altura).                                                | inteiro (padrão 1)                                                      |
| `columnspan`   | Número de colunas que o widget ocupará (largura).                                              | inteiro (padrão 1)                                                      |
| `sticky`       | Define a direção(ões) em que o widget se expande dentro da célula. Valores compostos.         | `"n"`, `"s"`, `"e"`, `"w"`, combinações (ex: `"ew"`)                   |
| `padx`, `pady` | Espaçamento externo (padding) ao redor do widget, em pixels.                                   | inteiro ou tupla                                                         |
| `ipadx`, `ipady`| Espaçamento interno (padding) dentro do widget, aumentando seu tamanho.                      | inteiro ou tupla                                                         |

### 3.3. Configurando Linhas e Colunas

Além das opções nos widgets, o container (como `CTkFrame` ou `CTk`) oferece métodos para configurar o comportamento das linhas e colunas:

| Método                         | Descrição                                                                                     |
|--------------------------------|-----------------------------------------------------------------------------------------------|
| `columnconfigure(index, weight)`| Define um peso (weight) para a coluna, determinando como o espaço extra é distribuído.        |
| `rowconfigure(index, weight)`   | Define um peso para a linha.                                                                  |
| `grid_columnconfigure(index, weight)` | Sinônimo de `columnconfigure` (presente em alguns widgets).                               |
| `grid_rowconfigure(index, weight)`    | Sinônimo de `rowconfigure`.                                                               |

### 3.4. Explicação Detalhada de Cada Opção

#### **`row` e `column`**
Especificam a posição do widget na grade. Linhas e colunas não precisam ser contíguas; células vazias são permitidas.

```python
label = ctk.CTkLabel(janela, text="Nome:")
label.grid(row=0, column=0, padx=5, pady=5)

entry = ctk.CTkEntry(janela)
entry.grid(row=0, column=1, padx=5, pady=5)
```

#### **`rowspan` e `columnspan`**
Permitem que um widget ocupe múltiplas células.

```python
# Uma imagem que ocupa 2 linhas e 3 colunas
imagem_label.grid(row=0, column=0, rowspan=2, columnspan=3)
```

#### **`sticky`**
Define como o widget se comporta quando a célula é maior que o widget. Pode ser uma string com os pontos cardeais que indicam a quais bordas o widget deve "grudar". Combinações comuns:

- `"w"` (west): gruda na esquerda.
- `"e"` (east): gruda na direita.
- `"n"` (north): gruda no topo.
- `"s"` (south): gruda na base.
- `"we"` ou `"ew"`: expande horizontalmente (preenche largura).
- `"ns"`: expande verticalmente.
- `"nsew"`: expande em ambas as direções (preenche toda a célula).

```python
# Botão que preenche horizontalmente a célula
btn.grid(row=1, column=0, columnspan=2, sticky="ew")
```

#### **`padx`, `pady` e `ipadx`, `ipady`**
Funcionam da mesma forma que no `pack`: `padx/pady` são margens externas à célula; `ipadx/ipady` são margens internas (aumentam o tamanho do widget).

```python
entry.grid(row=0, column=1, padx=10, pady=5, ipadx=5)
```

#### **Controlando o Espaço com Peso (Weight)**

Por padrão, as linhas e colunas não se expandem quando a janela é redimensionada. Para que isso aconteça, devemos atribuir **pesos** (weights) com `columnconfigure` e `rowconfigure`. O peso determina a proporção do espaço extra que aquela linha ou coluna receberá.

```python
# A coluna 1 (onde está o entry) receberá todo o espaço extra horizontal
janela.columnconfigure(1, weight=1)

# A linha 2 (onde está um botão) receberá todo o espaço extra vertical
janela.rowconfigure(2, weight=1)
```

**Exemplo completo com pesos:**

```python
janela = ctk.CTk()
janela.geometry("400x300")

# Configurar pesos
janela.columnconfigure(0, weight=1)  # coluna 0 expande
janela.columnconfigure(1, weight=2)  # coluna 1 expande 2x mais
janela.rowconfigure(0, weight=1)     # linha 0 expande

# Widgets
label1 = ctk.CTkLabel(janela, text="Col 0")
label1.grid(row=0, column=0, sticky="nsew")

label2 = ctk.CTkLabel(janela, text="Col 1")
label2.grid(row=0, column=1, sticky="nsew")

# Botão fixo na parte inferior
btn = ctk.CTkButton(janela, text="Botão")
btn.grid(row=1, column=0, columnspan=2, sticky="ew")

janela.mainloop()
```

### 3.5. Manipulação Dinâmica da Grade

Você pode adicionar ou remover widgets da grade a qualquer momento. Para remover um widget, use `grid_forget()` ou `grid_remove()` (o último mantém as configurações de grade para reaparecer depois). Para reposicionar, basta chamar `grid()` novamente com novas opções.

```python
# Esconder temporariamente
widget.grid_remove()

# Mostrar novamente na mesma posição
widget.grid()

# Mudar de posição
widget.grid(row=2, column=1)
```

---

## 4. Comparação entre `pack` e `grid`

| Aspecto               | `pack`                                            | `grid`                                         |
|-----------------------|---------------------------------------------------|------------------------------------------------|
| **Complexidade**      | Simples, ideal para layouts lineares              | Mais poderoso, ideal para layouts tabulares    |
| **Controle preciso**  | Menos controle sobre posição exata                | Posicionamento exato por coordenadas de grade  |
| **Responsividade**    | Via `fill` e `expand`                             | Via `sticky` e pesos (`columnconfigure`)       |
| **Mistura**           | Não pode misturar `pack` e `grid` no mesmo container | Não pode misturar `pack` e `grid` no mesmo container |
| **Uso comum**         | Barras de ferramentas, menus verticais, listas    | Formulários, painéis com múltiplos componentes |

### 4.1. Atenção: Não Misture em um Mesmo Container

Em um mesmo container (frame ou janela), você **não pode** usar `pack` e `grid` simultaneamente. Isso causará um erro de layout. Se precisar de diferentes gerenciadores, organize os widgets em subframes, cada um com seu próprio gerenciador.

```python
# Correto: subframes isolados
frame1 = ctk.CTkFrame(janela)
frame1.pack(fill="x")

frame2 = ctk.CTkFrame(janela)
frame2.pack(fill="both", expand=True)

# frame1 pode usar grid internamente
label = ctk.CTkLabel(frame1, text="Grid dentro de pack")
label.grid(row=0, column=0)

# frame2 pode usar pack internamente
btn = ctk.CTkButton(frame2, text="Pack dentro de grid? Não, aqui é pack")
btn.pack()
```

---

## 5. Exemplos Práticos Avançados

### 5.1. Exemplo 1: Formulário de Cadastro com Grid

```python
import customtkinter as ctk

ctk.set_appearance_mode("dark")
ctk.set_default_color_theme("blue")

class Formulario(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Formulário de Cadastro")
        self.geometry("500x400")

        # Frame principal para organizar conteúdo
        self.frame = ctk.CTkFrame(self)
        self.frame.pack(fill="both", expand=True, padx=20, pady=20)

        # Configurar pesos para que os campos se expandam horizontalmente
        self.frame.columnconfigure(1, weight=1)  # coluna dos campos de entrada

        # Campos do formulário
        self.criar_campo("Nome:", 0)
        self.criar_campo("E-mail:", 1)
        self.criar_campo("Senha:", 2, show="*")

        # Checkbox
        self.check = ctk.CTkCheckBox(self.frame, text="Aceito os termos")
        self.check.grid(row=3, column=0, columnspan=2, pady=10)

        # Botões
        btn_salvar = ctk.CTkButton(self.frame, text="Salvar", command=self.salvar)
        btn_salvar.grid(row=4, column=0, columnspan=2, pady=10, sticky="ew")

        # Label de mensagem
        self.msg = ctk.CTkLabel(self.frame, text="", text_color="green")
        self.msg.grid(row=5, column=0, columnspan=2)

    def criar_campo(self, texto, linha, **kwargs):
        label = ctk.CTkLabel(self.frame, text=texto)
        label.grid(row=linha, column=0, padx=5, pady=5, sticky="w")

        entry = ctk.CTkEntry(self.frame, **kwargs)
        entry.grid(row=linha, column=1, padx=5, pady=5, sticky="ew")
        return entry

    def salvar(self):
        self.msg.configure(text="Cadastro realizado com sucesso!")

if __name__ == "__main__":
    app = Formulario()
    app.mainloop()
```

### 5.2. Exemplo 2: Painel com Barra Lateral (pack) e Área Principal (grid)

```python
import customtkinter as ctk

class App(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Dashboard")
        self.geometry("800x500")

        # Frame da barra lateral (pack)
        self.sidebar = ctk.CTkFrame(self, width=200, corner_radius=0)
        self.sidebar.pack(side="left", fill="y")

        # Botões na sidebar (pack vertical)
        btn_inicio = ctk.CTkButton(self.sidebar, text="Início", command=self.mostrar_inicio)
        btn_inicio.pack(pady=10, padx=10, fill="x")

        btn_config = ctk.CTkButton(self.sidebar, text="Configurações", command=self.mostrar_config)
        btn_config.pack(pady=10, padx=10, fill="x")

        # Frame do conteúdo principal (grid)
        self.conteudo = ctk.CTkFrame(self)
        self.conteudo.pack(side="right", fill="both", expand=True)

        # Configurar grid do conteúdo
        self.conteudo.columnconfigure(0, weight=1)
        self.conteudo.rowconfigure(0, weight=1)

        # Widgets iniciais
        self.mostrar_inicio()

    def mostrar_inicio(self):
        self.limpar_conteudo()
        label = ctk.CTkLabel(self.conteudo, text="Bem-vindo!", font=("Arial", 24))
        label.grid(row=0, column=0, sticky="nsew")

    def mostrar_config(self):
        self.limpar_conteudo()
        # Simula um formulário de configurações dentro do grid
        label = ctk.CTkLabel(self.conteudo, text="Configurações", font=("Arial", 20))
        label.grid(row=0, column=0, pady=10)

        # Configurar pesos para expansão
        self.conteudo.columnconfigure(0, weight=1)
        self.conteudo.rowconfigure(1, weight=1)

        # Frame para opções
        opcoes_frame = ctk.CTkFrame(self.conteudo)
        opcoes_frame.grid(row=1, column=0, sticky="nsew", padx=20, pady=10)

        opcoes_frame.columnconfigure(0, weight=1)
        ctk.CTkCheckBox(opcoes_frame, text="Opção 1").grid(row=0, column=0, sticky="w")
        ctk.CTkCheckBox(opcoes_frame, text="Opção 2").grid(row=1, column=0, sticky="w")

    def limpar_conteudo(self):
        for widget in self.conteudo.winfo_children():
            widget.destroy()

if __name__ == "__main__":
    app = App()
    app.mainloop()
```

### 5.3. Exemplo 3: Layout Responsivo com Grid e Pesos

```python
janela = ctk.CTk()
janela.title("Layout Responsivo")
janela.geometry("600x400")

# Configurar pesos para que o conteúdo se expanda
janela.columnconfigure(0, weight=1)
janela.rowconfigure(0, weight=1)
janela.rowconfigure(1, weight=0)  # linha do botão não expande

# Área principal (grid)
frame_principal = ctk.CTkFrame(janela)
frame_principal.grid(row=0, column=0, sticky="nsew", padx=10, pady=10)

# Configurar grade interna da área principal
frame_principal.columnconfigure(0, weight=1)
frame_principal.columnconfigure(1, weight=2)
frame_principal.rowconfigure(0, weight=1)
frame_principal.rowconfigure(1, weight=1)

# Widgets dentro da área principal
label1 = ctk.CTkLabel(frame_principal, text="Esquerda", fg_color="gray")
label1.grid(row=0, column=0, sticky="nsew", padx=2, pady=2)

label2 = ctk.CTkLabel(frame_principal, text="Direita Superior", fg_color="lightblue")
label2.grid(row=0, column=1, sticky="nsew", padx=2, pady=2)

label3 = ctk.CTkLabel(frame_principal, text="Direita Inferior", fg_color="lightgreen")
label3.grid(row=1, column=1, sticky="nsew", padx=2, pady=2)

# Botão na parte inferior (não expande)
btn = ctk.CTkButton(janela, text="Clique aqui")
btn.grid(row=1, column=0, pady=10)

janela.mainloop()
```

---

## 6. Boas Práticas e Dicas

1. **Escolha o gerenciador adequado:**
   - Use `pack` para layouts lineares (barras, listas de botões).
   - Use `grid` para layouts tabulares (formulários, painéis complexos).
   - Use `place` apenas para posicionamento absoluto quando necessário (raramente).

2. **Nunca misture `pack` e `grid` no mesmo container.** Se precisar, crie subcontainers (frames) e use cada gerenciador em seu respectivo container.

3. **Defina pesos (`weight`) para que o layout seja responsivo.** Sempre que a janela puder ser redimensionada, configure `columnconfigure` e `rowconfigure`.

4. **Utilize `sticky` no grid para expandir widgets.** Combinado com pesos, `sticky="nsew"` faz o widget preencher toda a célula.

5. **Organize o código:**
   - Separe a criação da interface em funções ou classes.
   - Agrupe widgets relacionados em frames.

6. **Use `pack` com `fill` e `expand` para criar áreas expansíveis.** Por exemplo, uma barra superior fixa e um conteúdo central que se expande.

7. **Prefira `grid` para formulários:** ele permite alinhar labels e campos de entrada facilmente.

8. **Teste com diferentes tamanhos de janela** para garantir que o layout se comporta como esperado.

---
