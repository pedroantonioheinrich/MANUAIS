# Manual Completo de GTK em C
## Aprenda a Criar Interfaces Gráficas Profissionais

---

## Sumário
1. [Introdução ao GTK](#introdução)
2. [Configuração do Ambiente](#configuracao)
3. [Primeiro Programa: "Hello World"](#primeiro-programa)
4. [Widgets Básicos](#widgets-basicos)
5. [Gerenciamento de Layout](#layout)
6. [Sinais e Callbacks](#sinais)
7. [Widgets Avançados](#widgets-avancados)
8. [Desenhando com Cairo](#cairo)
9. [Projeto Prático](#projeto)
10. [Dicas e Boas Práticas](#dicas)

---

## 1. Introdução ao GTK {#introducao}

### O que é GTK?
GTK (GIMP Toolkit) é um kit de ferramentas multiplataforma para criar interfaces gráficas. Originalmente desenvolvido para o GIMP, hoje é usado em aplicativos como Inkscape, Gedit e muitos outros.

### Filosofia do GTK
- **Orientado a objetos** (mesmo sendo em C)
- **Widgets são objetos** que emitem sinais
- **Hierarquia de containers** para organização
- **Separação entre lógica e apresentação**

### Estrutura Básica de um Programa GTK
Todo programa GTK segue este fluxo:
1. Inicializar o GTK
2. Criar widgets
3. Conectar sinais
4. Mostrar tudo
5. Loop principal

---

## 2. Configuração do Ambiente {#configuracao}

### Instalação no Linux (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install libgtk-3-dev pkg-config
```

### Instalação no Linux (Fedora)
```bash
sudo dnf install gtk3-devel
```

### Instalação no Windows
1. Baixe MSYS2 do site oficial
2. No terminal MSYS2:
```bash
pacman -S mingw-w64-x86_64-gtk3
```

### Compilando Programas GTK
```bash
# Comando básico de compilação
gcc programa.c -o programa `pkg-config --cflags --libs gtk+-3.0`

# Com mais flags de warning
gcc -Wall -g programa.c -o programa `pkg-config --cflags --libs gtk+-3.0`
```

---

## 3. Primeiro Programa: "Hello World" {#primeiro-programa}

Vamos criar nosso primeiro programa GTK, analisando cada linha detalhadamente:

```c
#include <gtk/gtk.h>

// Função callback chamada quando clicam no botão
static void hello_world(GtkWidget *widget, gpointer data) {
    g_print("Hello World\n");
}

// Função callback para fechar a janela
static void destroy(GtkWidget *widget, gpointer data) {
    gtk_main_quit();
}

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *button;
    
    // 1. Inicializa o GTK
    gtk_init(&argc, &argv);
    
    // 2. Cria a janela principal
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    
    // 3. Configura propriedades da janela
    gtk_window_set_title(GTK_WINDOW(window), "Meu Primeiro Programa");
    gtk_window_set_default_size(GTK_WINDOW(window), 300, 200);
    gtk_container_set_border_width(GTK_CONTAINER(window), 10);
    
    // 4. Conecta o sinal de destroy
    g_signal_connect(window, "destroy", G_CALLBACK(destroy), NULL);
    
    // 5. Cria um botão
    button = gtk_button_new_with_label("Clique Aqui");
    
    // 6. Conecta o sinal do botão
    g_signal_connect(button, "clicked", G_CALLBACK(hello_world), NULL);
    
    // 7. Adiciona o botão à janela
    gtk_container_add(GTK_CONTAINER(window), button);
    
    // 8. Mostra todos os widgets
    gtk_widget_show_all(window);
    
    // 9. Inicia o loop principal
    gtk_main();
    
    return 0;
}
```

### Explicação Detalhada:

#### `#include <gtk/gtk.h>`
Inclui todas as funções e definições necessárias do GTK.

#### `gtk_init(&argc, &argv)`
- **Propósito**: Inicializa o GTK
- **Parâmetros**: 
  - `argc`: ponteiro para contagem de argumentos
  - `argv`: ponteiro para array de argumentos
- **O que faz**: Configura o ambiente, carrega temas, inicializa subsystems

#### `gtk_window_new(GTK_WINDOW_TOPLEVEL)`
- **Propósito**: Cria uma nova janela
- **Parâmetro**: Tipo de janela
  - `GTK_WINDOW_TOPLEVEL`: janela padrão com bordas
  - `GTK_WINDOW_POPUP`: janela sem decorações
- **Retorno**: GtkWidget* (ponteiro para o widget)

#### Macros de Casting
- `GTK_WINDOW(window)`: converte GtkWidget* para GtkWindow*
- `GTK_CONTAINER(window)`: converte para container
- **Por que usar**: GTK usa herança em C, essas macros fazem type checking seguro

#### `g_signal_connect()`
- **Propósito**: Conecta um sinal a uma função callback
- **Parâmetros**:
  1. Instância do objeto
  2. Nome do sinal (string)
  3. Função callback (com cast G_CALLBACK)
  4. Dados do usuário (pode ser NULL)

#### `gtk_main()`
- **Propósito**: Inicia o loop principal do GTK
- **Comportamento**: Fica em loop até `gtk_main_quit()` ser chamado
- **Processa**: Eventos do mouse, teclado, redesenho, etc.

---

## 4. Widgets Básicos {#widgets-basicos}

### 4.1 Botões (GtkButton)

```c
#include <gtk/gtk.h>

void on_button_clicked(GtkButton *button, gpointer user_data) {
    const gchar *text = gtk_button_get_label(button);
    g_print("Botão '%s' clicado!\n", text);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Botões");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    // Container vertical
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // 1. Botão simples com texto
    GtkWidget *btn1 = gtk_button_new_with_label("Botão Simples");
    g_signal_connect(btn1, "clicked", G_CALLBACK(on_button_clicked), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn1, TRUE, TRUE, 0);
    
    // 2. Botão com ícone
    GtkWidget *btn2 = gtk_button_new();
    GtkWidget *box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    GtkWidget *icon = gtk_image_new_from_icon_name("document-save", 
                                                    GTK_ICON_SIZE_BUTTON);
    GtkWidget *label = gtk_label_new("Salvar");
    
    gtk_box_pack_start(GTK_BOX(box), icon, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(box), label, FALSE, FALSE, 0);
    gtk_container_add(GTK_CONTAINER(btn2), box);
    
    g_signal_connect(btn2, "clicked", G_CALLBACK(on_button_clicked), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn2, TRUE, TRUE, 0);
    
    // 3. Botão de toggle (liga/desliga)
    GtkWidget *btn3 = gtk_toggle_button_new_with_label("Modo Edição");
    g_signal_connect(btn3, "toggled", G_CALLBACK(on_toggle_changed), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn3, TRUE, TRUE, 0);
    
    // 4. Botão de rádio
    GSList *group = NULL;
    GtkWidget *radio1 = gtk_radio_button_new_with_label(group, "Opção 1");
    group = gtk_radio_button_get_group(GTK_RADIO_BUTTON(radio1));
    GtkWidget *radio2 = gtk_radio_button_new_with_label(group, "Opção 2");
    GtkWidget *radio3 = gtk_radio_button_new_with_label(group, "Opção 3");
    
    gtk_box_pack_start(GTK_BOX(vbox), radio1, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(vbox), radio2, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(vbox), radio3, TRUE, TRUE, 0);
    
    // 5. Botão com link
    GtkWidget *btn_link = gtk_link_button_new("https://www.gtk.org");
    gtk_box_pack_start(GTK_BOX(vbox), btn_link, TRUE, TRUE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}

void on_toggle_changed(GtkToggleButton *button, gpointer user_data) {
    if (gtk_toggle_button_get_active(button)) {
        g_print("Toggle ativado\n");
    } else {
        g_print("Toggle desativado\n");
    }
}
```

### 4.2 Rótulos (GtkLabel)

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Labels");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 20);
    
    // 1. Label simples
    GtkWidget *label1 = gtk_label_new("Texto simples");
    gtk_box_pack_start(GTK_BOX(vbox), label1, FALSE, FALSE, 0);
    
    // 2. Label com formatação (Pango Markup)
    GtkWidget *label2 = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(label2), 
        "<span foreground='blue' size='x-large'><b>Texto em negrito azul</b></span>");
    gtk_box_pack_start(GTK_BOX(vbox), label2, FALSE, FALSE, 0);
    
    // 3. Label com múltiplas linhas
    GtkWidget *label3 = gtk_label_new(
        "Esta é uma label com múltiplas linhas\n"
        "Ela quebra automaticamente o texto\n"
        "Usando \\n para novas linhas"
    );
    gtk_box_pack_start(GTK_BOX(vbox), label3, FALSE, FALSE, 0);
    
    // 4. Label com justificação
    GtkWidget *label4 = gtk_label_new(
        "Este texto é justificado. Quando a janela for redimensionada, "
        "o texto será quebrado automaticamente e alinhado conforme a "
        "configuração de justificação escolhida."
    );
    gtk_label_set_justify(GTK_LABEL(label4), GTK_JUSTIFY_CENTER);
    gtk_label_set_line_wrap(GTK_LABEL(label4), TRUE);
    gtk_label_set_max_width_chars(GTK_LABEL(label4), 40);
    gtk_box_pack_start(GTK_BOX(vbox), label4, FALSE, FALSE, 0);
    
    // 5. Label com selecionável (para copiar texto)
    GtkWidget *label5 = gtk_label_new("Este texto pode ser selecionado e copiado");
    gtk_label_set_selectable(GTK_LABEL(label5), TRUE);
    gtk_box_pack_start(GTK_BOX(vbox), label5, FALSE, FALSE, 0);
    
    // 6. Label com ângulo (texto rotacionado)
    GtkWidget *label6 = gtk_label_new("Texto Rotacionado");
    gtk_label_set_angle(GTK_LABEL(label6), 45);
    gtk_box_pack_start(GTK_BOX(vbox), label6, FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 4.3 Entradas de Texto (GtkEntry)

```c
#include <gtk/gtk.h>

void on_entry_activate(GtkEntry *entry, gpointer user_data) {
    const gchar *text = gtk_entry_get_text(entry);
    g_print("Texto inserido: %s\n", text);
}

void on_toggle_visibility(GtkToggleButton *button, gpointer user_data) {
    GtkEntry *entry = GTK_ENTRY(user_data);
    gboolean active = gtk_toggle_button_get_active(button);
    gtk_entry_set_visibility(entry, !active);  // !active porque é "Ocultar"
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Entry");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 20);
    
    // 1. Entry simples
    GtkWidget *entry1 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry1), "Digite algo...");
    gtk_box_pack_start(GTK_BOX(vbox), entry1, FALSE, FALSE, 0);
    g_signal_connect(entry1, "activate", G_CALLBACK(on_entry_activate), NULL);
    
    // 2. Entry com texto pré-definido
    GtkWidget *entry2 = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(entry2), "Texto inicial");
    gtk_box_pack_start(GTK_BOX(vbox), entry2, FALSE, FALSE, 0);
    
    // 3. Entry para senha
    GtkWidget *entry3 = gtk_entry_new();
    gtk_entry_set_visibility(GTK_ENTRY(entry3), FALSE);
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry3), "Senha");
    gtk_box_pack_start(GTK_BOX(vbox), entry3, FALSE, FALSE, 0);
    
    // Botão para mostrar/ocultar senha
    GtkWidget *check = gtk_check_button_new_with_label("Ocultar senha");
    gtk_toggle_button_set_active(GTK_TOGGLE_BUTTON(check), TRUE);
    g_signal_connect(check, "toggled", G_CALLBACK(on_toggle_visibility), entry3);
    gtk_box_pack_start(GTK_BOX(vbox), check, FALSE, FALSE, 0);
    
    // 4. Entry com limitação de caracteres
    GtkWidget *entry4 = gtk_entry_new();
    gtk_entry_set_max_length(GTK_ENTRY(entry4), 10);
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry4), "Máx 10 caracteres");
    gtk_box_pack_start(GTK_BOX(vbox), entry4, FALSE, FALSE, 0);
    
    // 5. Entry com ícone
    GtkWidget *entry5 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry5), "Buscar...");
    gtk_entry_set_icon_from_icon_name(GTK_ENTRY(entry5), 
                                       GTK_ENTRY_ICON_PRIMARY, 
                                       "system-search");
    gtk_box_pack_start(GTK_BOX(vbox), entry5, FALSE, FALSE, 0);
    
    // 6. Entry numérico
    GtkWidget *entry6 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry6), "Números apenas");
    gtk_entry_set_input_purpose(GTK_ENTRY(entry6), GTK_INPUT_PURPOSE_DIGITS);
    gtk_box_pack_start(GTK_BOX(vbox), entry6, FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

---

## 5. Gerenciamento de Layout {#layout}

### 5.1 Box (GtkBox) - Layout Linear

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Box");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    // Container principal: vertical
    GtkWidget *main_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), main_vbox);
    gtk_container_set_border_width(GTK_CONTAINER(main_vbox), 10);
    
    // Título
    GtkWidget *title = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(title), 
        "<span size='xx-large' weight='bold'>Layout com Box</span>");
    gtk_box_pack_start(GTK_BOX(main_vbox), title, FALSE, FALSE, 10);
    
    // Linha horizontal 1: Botões alinhados à esquerda
    GtkWidget *hbox1 = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(main_vbox), hbox1, FALSE, FALSE, 0);
    
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 1"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 2"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 3"), FALSE, FALSE, 0);
    
    // Linha horizontal 2: Botões expandindo
    GtkWidget *hbox2 = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(main_vbox), hbox2, FALSE, FALSE, 0);
    
    GtkWidget *btn_expand1 = gtk_button_new_with_label("Expande");
    GtkWidget *btn_expand2 = gtk_button_new_with_label("Expande também");
    GtkWidget *btn_fixo = gtk_button_new_with_label("Fixo");
    
    gtk_box_pack_start(GTK_BOX(hbox2), btn_expand1, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox2), btn_expand2, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox2), btn_fixo, FALSE, FALSE, 0);
    
    // Separador visual
    gtk_box_pack_start(GTK_BOX(main_vbox), 
        gtk_separator_new(GTK_ORIENTATION_HORIZONTAL), FALSE, FALSE, 10);
    
    // Área central com duas colunas
    GtkWidget *center_hbox = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 20);
    gtk_box_pack_start(GTK_BOX(main_vbox), center_hbox, TRUE, TRUE, 0);
    
    // Coluna esquerda (vertical)
    GtkWidget *left_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(center_hbox), left_vbox, TRUE, TRUE, 0);
    
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_label_new("Coluna Esquerda"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_button_new_with_label("Botão 1"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_button_new_with_label("Botão 2"), FALSE, FALSE, 0);
    
    // Coluna direita (vertical)
    GtkWidget *right_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(center_hbox), right_vbox, TRUE, TRUE, 0);
    
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_label_new("Coluna Direita"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_button_new_with_label("Botão A"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_button_new_with_label("Botão B"), FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 5.2 Grid (GtkGrid) - Layout em Tabela

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Grid");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    // Criando o grid
    GtkWidget *grid = gtk_grid_new();
    gtk_container_add(GTK_CONTAINER(window), grid);
    gtk_container_set_border_width(GTK_CONTAINER(grid), 10);
    gtk_grid_set_row_spacing(GTK_GRID(grid), 10);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 10);
    
    // Preenchendo o grid
    // Linha 0: Título em todas as colunas
    GtkWidget *title = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(title), 
        "<span size='large' weight='bold'>Formulário de Cadastro</span>");
    gtk_grid_attach(GTK_GRID(grid), title, 0, 0, 2, 1);
    
    // Linha 1: Campos
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Nome:"), 0, 1, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 1, 1, 1);
    
    // Linha 2: Email
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Email:"), 0, 2, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 2, 1, 1);
    
    // Linha 3: Telefone
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Telefone:"), 0, 3, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 3, 1, 1);
    
    // Linha 4: Sexo (usando duas colunas)
    GtkWidget *sexo_label = gtk_label_new("Sexo:");
    gtk_grid_attach(GTK_GRID(grid), sexo_label, 0, 4, 1, 1);
    
    GtkWidget *sexo_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Masculino"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Feminino"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Outro"), FALSE, FALSE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), sexo_box, 1, 4, 1, 1);
    
    // Linha 5: Interesses (checkboxes)
    GtkWidget *interesses_label = gtk_label_new("Interesses:");
    gtk_grid_attach(GTK_GRID(grid), interesses_label, 0, 5, 1, 1);
    
    GtkWidget *interesses_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Programação"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Design"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Música"), FALSE, FALSE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), interesses_box, 1, 5, 1, 1);
    
    // Linha 6: Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Salvar"), TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Cancelar"), TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Limpar"), TRUE, TRUE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), button_box, 0, 6, 2, 1);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 5.3 Paned (GtkPaned) - Divisores Redimensionáveis

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Paned");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    
    // Paned vertical (divide a janela em cima e baixo)
    GtkWidget *vpaned = gtk_paned_new(GTK_ORIENTATION_VERTICAL);
    gtk_container_add(GTK_CONTAINER(window), vpaned);
    
    // Parte superior - Paned horizontal
    GtkWidget *hpaned = gtk_paned_new(GTK_ORIENTATION_HORIZONTAL);
    gtk_paned_add1(GTK_PANED(vpaned), hpaned);
    
    // Lado esquerdo do paned horizontal
    GtkWidget *left_frame = gtk_frame_new("Navegação");
    gtk_frame_set_shadow_type(GTK_FRAME(left_frame), GTK_SHADOW_ETCHED_OUT);
    gtk_paned_add1(GTK_PANED(hpaned), left_frame);
    
    GtkWidget *left_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(left_frame), left_box);
    gtk_container_set_border_width(GTK_CONTAINER(left_box), 10);
    
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Página Inicial"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Arquivos"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Configurações"), FALSE, FALSE, 0);
    
    // Lado direito do paned horizontal
    GtkWidget *right_frame = gtk_frame_new("Conteúdo");
    gtk_paned_add2(GTK_PANED(hpaned), right_frame);
    
    GtkWidget *right_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(right_frame), right_box);
    gtk_container_set_border_width(GTK_CONTAINER(right_box), 10);
    
    gtk_box_pack_start(GTK_BOX(right_box), 
        gtk_label_new("Bem-vindo ao aplicativo!"), FALSE, FALSE, 0);
    
    // Parte inferior do paned vertical
    GtkWidget *bottom_frame = gtk_frame_new("Status");
    gtk_paned_add2(GTK_PANED(vpaned), bottom_frame);
    
    GtkWidget *status_label = gtk_label_new("Pronto");
    gtk_container_add(GTK_CONTAINER(bottom_frame), status_label);
    gtk_container_set_border_width(GTK_CONTAINER(bottom_frame), 10);
    
    // Definir posições iniciais
    gtk_paned_set_position(GTK_PANED(hpaned), 200);
    gtk_paned_set_position(GTK_PANED(vpaned), 300);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

---

## 6. Sinais e Callbacks {#sinais}

### 6.1 Tipos de Sinais Comuns

```c
#include <gtk/gtk.h>
#include <stdio.h>
#include <string.h>

// Estrutura para passar múltiplos dados
typedef struct {
    GtkWidget *entry;
    GtkWidget *label;
    GtkWidget *text_view;
} AppWidgets;

// Callback para clique do botão
void on_button_clicked(GtkButton *button, AppWidgets *widgets) {
    const gchar *text = gtk_entry_get_text(GTK_ENTRY(widgets->entry));
    
    if (strlen(text) > 0) {
        char markup[256];
        snprintf(markup, sizeof(markup), 
            "<span foreground='green' size='large'>%s</span>", text);
        gtk_label_set_markup(GTK_LABEL(widgets->label), markup);
        
        // Adicionar ao TextView
        GtkTextBuffer *buffer = gtk_text_view_get_buffer(
            GTK_TEXT_VIEW(widgets->text_view));
        GtkTextIter end;
        gtk_text_buffer_get_end_iter(buffer, &end);
        gtk_text_buffer_insert(buffer, &end, text, -1);
        gtk_text_buffer_insert(buffer, &end, "\n", -1);
    }
}

// Callback para mudança no entry
void on_entry_changed(GtkEditable *editable, AppWidgets *widgets) {
    const gchar *text = gtk_entry_get_text(GTK_ENTRY(editable));
    g_print("Texto mudou: %s\n", text);
}

// Callback para foco
gboolean on_entry_focus_in(GtkWidget *widget, GdkEventFocus *event, 
                           gpointer user_data) {
    g_print("Entry ganhou foco\n");
    gtk_widget_override_background_color(widget, GTK_STATE_FLAG_NORMAL,
        &(GdkRGBA){0.9, 0.9, 1.0, 1.0});
    return FALSE;
}

gboolean on_entry_focus_out(GtkWidget *widget, GdkEventFocus *event, 
                            gpointer user_data) {
    g_print("Entry perdeu foco\n");
    gtk_widget_override_background_color(widget, GTK_STATE_FLAG_NORMAL,
        &(GdkRGBA){1.0, 1.0, 1.0, 1.0});
    return FALSE;
}

// Callback para tecla pressionada
gboolean on_key_press(GtkWidget *widget, GdkEventKey *event, 
                      gpointer user_data) {
    g_print("Tecla pressionada: %s (código: %d)\n", 
            gdk_keyval_name(event->keyval), event->keyval);
    
    if (event->keyval == GDK_KEY_Escape) {
        gtk_main_quit();
        return TRUE;
    }
    return FALSE;
}

// Callback para quando a janela é fechada
gboolean on_window_delete_event(GtkWidget *widget, GdkEvent *event, 
                                gpointer user_data) {
    g_print("Janela sendo fechada...\n");
    // Retornar FALSE permite o fechamento, TRUE cancela
    return FALSE;
}

// Callback para redimensionamento da janela
void on_window_size_allocate(GtkWidget *widget, GdkRectangle *allocation,
                             gpointer user_data) {
    g_print("Janela redimensionada: %dx%d\n", 
            allocation->width, allocation->height);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    AppWidgets widgets;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Sinais");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    // Conectar sinais da janela
    g_signal_connect(window, "delete-event", 
                     G_CALLBACK(on_window_delete_event), NULL);
    g_signal_connect(window, "destroy", 
                     G_CALLBACK(gtk_main_quit), NULL);
    g_signal_connect(window, "size-allocate", 
                     G_CALLBACK(on_window_size_allocate), NULL);
    g_signal_connect(window, "key-press-event", 
                     G_CALLBACK(on_key_press), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Criar widgets
    widgets.entry = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(widgets.entry), 
                                   "Digite algo...");
    gtk_box_pack_start(GTK_BOX(vbox), widgets.entry, FALSE, FALSE, 0);
    
    // Conectar sinais do entry
    g_signal_connect(widgets.entry, "changed", 
                     G_CALLBACK(on_entry_changed), &widgets);
    g_signal_connect(widgets.entry, "focus-in-event", 
                     G_CALLBACK(on_entry_focus_in), NULL);
    g_signal_connect(widgets.entry, "focus-out-event", 
                     G_CALLBACK(on_entry_focus_out), NULL);
    
    // Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_box_pack_start(GTK_BOX(vbox), button_box, FALSE, FALSE, 0);
    
    GtkWidget *btn_ok = gtk_button_new_with_label("OK");
    gtk_box_pack_start(GTK_BOX(button_box), btn_ok, TRUE, TRUE, 0);
    
    GtkWidget *btn_clear = gtk_button_new_with_label("Limpar");
    gtk_box_pack_start(GTK_BOX(button_box), btn_clear, TRUE, TRUE, 0);
    
    // Label para mostrar resultado
    widgets.label = gtk_label_new(NULL);
    gtk_box_pack_start(GTK_BOX(vbox), widgets.label, FALSE, FALSE, 0);
    
    // TextView para log
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, GTK_POLICY_AUTOMATIC);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    widgets.text_view = gtk_text_view_new();
    gtk_text_view_set_editable(GTK_TEXT_VIEW(widgets.text_view), FALSE);
    gtk_container_add(GTK_CONTAINER(scrolled), widgets.text_view);
    
    // Conectar botões
    g_signal_connect(btn_ok, "clicked", 
                     G_CALLBACK(on_button_clicked), &widgets);
    g_signal_connect_swapped(btn_clear, "clicked", 
                             G_CALLBACK(gtk_entry_set_text), 
                             widgets.entry);
    g_signal_connect_swapped(btn_clear, "clicked", 
                             G_CALLBACK(gtk_label_set_text), 
                             widgets.label);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

### 6.2 Trabalhando com Múltiplos Dados

```c
#include <gtk/gtk.h>
#include <stdlib.h>

// Estrutura complexa para dados do usuário
typedef struct {
    gchar *nome;
    gint idade;
    gchar *email;
    GtkWidget *entry_nome;
    GtkWidget *spin_idade;
    GtkWidget *entry_email;
} Usuario;

// Função para liberar memória
void usuario_free(Usuario *user) {
    if (user) {
        g_free(user->nome);
        g_free(user->email);
        g_free(user);
    }
}

// Callback para salvar dados
void on_salvar_clicked(GtkButton *button, Usuario *user) {
    // Atualizar dados da estrutura
    if (user->nome) g_free(user->nome);
    user->nome = g_strdup(gtk_entry_get_text(GTK_ENTRY(user->entry_nome)));
    
    user->idade = gtk_spin_button_get_value_as_int(
        GTK_SPIN_BUTTON(user->spin_idade));
    
    if (user->email) g_free(user->email);
    user->email = g_strdup(gtk_entry_get_text(GTK_ENTRY(user->entry_email)));
    
    g_print("Usuário salvo:\n");
    g_print("  Nome: %s\n", user->nome);
    g_print("  Idade: %d\n", user->idade);
    g_print("  Email: %s\n", user->email);
}

// Callback para mostrar dados em dialog
void on_mostrar_clicked(GtkButton *button, Usuario *user) {
    GtkWidget *dialog;
    gchar *message;
    
    message = g_strdup_printf(
        "<b>Dados do Usuário:</b>\n\n"
        "Nome: %s\n"
        "Idade: %d\n"
        "Email: %s",
        user->nome ? user->nome : "(não definido)",
        user->idade,
        user->email ? user->email : "(não definido)");
    
    dialog = gtk_message_dialog_new(
        GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(button))),
        GTK_DIALOG_DESTROY_WITH_PARENT,
        GTK_MESSAGE_INFO,
        GTK_BUTTONS_OK,
        "%s", message);
    
    gtk_message_dialog_set_markup(GTK_MESSAGE_DIALOG(dialog), message);
    
    g_signal_connect(dialog, "response", 
                     G_CALLBACK(gtk_widget_destroy), NULL);
    
    gtk_widget_show_all(dialog);
    g_free(message);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Alocar e inicializar estrutura do usuário
    Usuario *user = g_new0(Usuario, 1);
    user->nome = g_strdup("João Silva");
    user->idade = 25;
    user->email = g_strdup("joao@email.com");
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Dados do Usuário");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 250);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Quando a janela for destruída, liberar memória do usuário
    g_signal_connect_swapped(window, "destroy", 
                             G_CALLBACK(usuario_free), user);
    
    GtkWidget *grid = gtk_grid_new();
    gtk_container_add(GTK_CONTAINER(window), grid);
    gtk_container_set_border_width(GTK_CONTAINER(grid), 10);
    gtk_grid_set_row_spacing(GTK_GRID(grid), 5);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 5);
    
    // Criar e armazenar referências aos widgets
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Nome:"), 0, 0, 1, 1);
    user->entry_nome = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(user->entry_nome), user->nome);
    gtk_grid_attach(GTK_GRID(grid), user->entry_nome, 1, 0, 1, 1);
    
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Idade:"), 0, 1, 1, 1);
    user->spin_idade = gtk_spin_button_new_with_range(0, 120, 1);
    gtk_spin_button_set_value(GTK_SPIN_BUTTON(user->spin_idade), user->idade);
    gtk_grid_attach(GTK_GRID(grid), user->spin_idade, 1, 1, 1, 1);
    
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Email:"), 0, 2, 1, 1);
    user->entry_email = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(user->entry_email), user->email);
    gtk_grid_attach(GTK_GRID(grid), user->entry_email, 1, 2, 1, 1);
    
    // Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_grid_attach(GTK_GRID(grid), button_box, 0, 3, 2, 1);
    
    GtkWidget *btn_salvar = gtk_button_new_with_label("Salvar");
    GtkWidget *btn_mostrar = gtk_button_new_with_label("Mostrar");
    GtkWidget *btn_sair = gtk_button_new_with_label("Sair");
    
    gtk_box_pack_start(GTK_BOX(button_box), btn_salvar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_mostrar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_sair, TRUE, TRUE, 0);
    
    // Conectar sinais passando a estrutura do usuário
    g_signal_connect(btn_salvar, "clicked", G_CALLBACK(on_salvar_clicked), user);
    g_signal_connect(btn_mostrar, "clicked", G_CALLBACK(on_mostrar_clicked), user);
    g_signal_connect(btn_sair, "clicked", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

---

## 7. Widgets Avançados {#widgets-avancados}

### 7.1 TreeView (Listas e Árvores)

```c
#include <gtk/gtk.h>

// Estrutura para dados da lista
enum {
    COLUNA_NOME,
    COLUNA_IDADE,
    COLUNA_CIDADE,
    COLUNA_ATIVO,
    NUM_COLUNAS
};

// Função para adicionar uma pessoa à lista
void adicionar_pessoa(GtkListStore *store, const gchar *nome, 
                      gint idade, const gchar *cidade, gboolean ativo) {
    GtkTreeIter iter;
    gtk_list_store_append(store, &iter);
    gtk_list_store_set(store, &iter,
                       COLUNA_NOME, nome,
                       COLUNA_IDADE, idade,
                       COLUNA_CIDADE, cidade,
                       COLUNA_ATIVO, ativo,
                       -1);
}

// Callback para clique na árvore
void on_tree_row_activated(GtkTreeView *tree_view, GtkTreePath *path,
                           GtkTreeViewColumn *column, gpointer user_data) {
    GtkTreeModel *model = gtk_tree_view_get_model(tree_view);
    GtkTreeIter iter;
    
    if (gtk_tree_model_get_iter(model, &iter, path)) {
        gchar *nome;
        gint idade;
        gchar *cidade;
        gboolean ativo;
        
        gtk_tree_model_get(model, &iter,
                          COLUNA_NOME, &nome,
                          COLUNA_IDADE, &idade,
                          COLUNA_CIDADE, &cidade,
                          COLUNA_ATIVO, &ativo,
                          -1);
        
        g_print("Selecionado: %s, %d anos, %s, %s\n",
                nome, idade, cidade, ativo ? "Ativo" : "Inativo");
        
        g_free(nome);
        g_free(cidade);
    }
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo TreeView");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 300);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Criar o modelo de dados (ListStore)
    GtkListStore *store = gtk_list_store_new(NUM_COLUNAS,
                                             G_TYPE_STRING,
                                             G_TYPE_INT,
                                             G_TYPE_STRING,
                                             G_TYPE_BOOLEAN);
    
    // Adicionar dados
    adicionar_pessoa(store, "João Silva", 30, "São Paulo", TRUE);
    adicionar_pessoa(store, "Maria Santos", 25, "Rio de Janeiro", TRUE);
    adicionar_pessoa(store, "Pedro Oliveira", 35, "Belo Horizonte", FALSE);
    adicionar_pessoa(store, "Ana Costa", 28, "Curitiba", TRUE);
    adicionar_pessoa(store, "Carlos Souza", 42, "Porto Alegre", FALSE);
    
    // Criar a TreeView
    GtkWidget *tree_view = gtk_tree_view_new_with_model(GTK_TREE_MODEL(store));
    g_object_unref(store); // A tree_view agora tem sua própria referência
    
    // Configurar colunas
    // Coluna Nome
    GtkCellRenderer *renderer = gtk_cell_renderer_text_new();
    GtkTreeViewColumn *column = gtk_tree_view_column_new_with_attributes(
        "Nome", renderer, "text", COLUNA_NOME, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Idade
    renderer = gtk_cell_renderer_text_new();
    column = gtk_tree_view_column_new_with_attributes(
        "Idade", renderer, "text", COLUNA_IDADE, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Cidade
    renderer = gtk_cell_renderer_text_new();
    column = gtk_tree_view_column_new_with_attributes(
        "Cidade", renderer, "text", COLUNA_CIDADE, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Ativo (com toggle)
    renderer = gtk_cell_renderer_toggle_new();
    g_signal_connect(renderer, "toggled", 
                     G_CALLBACK(on_cell_toggled), store);
    column = gtk_tree_view_column_new_with_attributes(
        "Ativo", renderer, "active", COLUNA_ATIVO, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Conectar sinal de ativação de linha
    g_signal_connect(tree_view, "row-activated",
                     G_CALLBACK(on_tree_row_activated), NULL);
    
    // Adicionar à janela com scroll
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, 
                                   GTK_POLICY_AUTOMATIC);
    gtk_container_add(GTK_CONTAINER(scrolled), tree_view);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    // Botões de controle
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_box_pack_start(GTK_BOX(vbox), button_box, FALSE, FALSE, 0);
    
    GtkWidget *btn_adicionar = gtk_button_new_with_label("Adicionar");
    GtkWidget *btn_remover = gtk_button_new_with_label("Remover");
    GtkWidget *btn_propriedades = gtk_button_new_with_label("Propriedades");
    
    gtk_box_pack_start(GTK_BOX(button_box), btn_adicionar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_remover, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_propriedades, TRUE, TRUE, 0);
    
    // Conectar botões
    g_signal_connect(btn_adicionar, "clicked",
                     G_CALLBACK(on_adicionar_clicked), store);
    g_signal_connect(btn_remover, "clicked",
                     G_CALLBACK(on_remover_clicked), tree_view);
    g_signal_connect(btn_propriedades, "clicked",
                     G_CALLBACK(on_propriedades_clicked), tree_view);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}

// Callbacks para os botões
void on_cell_toggled(GtkCellRendererToggle *cell, gchar *path_str,
                     gpointer user_data) {
    GtkListStore *store = GTK_LIST_STORE(user_data);
    GtkTreeIter iter;
    GtkTreePath *path = gtk_tree_path_new_from_string(path_str);
    
    if (gtk_tree_model_get_iter(GTK_TREE_MODEL(store), &iter, path)) {
        gboolean ativo;
        gtk_tree_model_get(GTK_TREE_MODEL(store), &iter,
                          COLUNA_ATIVO, &ativo, -1);
        ativo = !ativo;
        gtk_list_store_set(store, &iter, COLUNA_ATIVO, ativo, -1);
    }
    
    gtk_tree_path_free(path);
}

void on_adicionar_clicked(GtkButton *button, GtkListStore *store) {
    // Simular adição de uma pessoa
    static int contador = 1;
    gchar *nome = g_strdup_printf("Novo Usuário %d", contador++);
    adicionar_pessoa(store, nome, 20 + (contador % 30), 
                     "Nova Cidade", TRUE);
    g_free(nome);
}

void on_remover_clicked(GtkButton *button, GtkTreeView *tree_view) {
    GtkTreeSelection *selection = gtk_tree_view_get_selection(tree_view);
    GtkTreeModel *model;
    GtkTreeIter iter;
    
    if (gtk_tree_selection_get_selected(selection, &model, &iter)) {
        gtk_list_store_remove(GTK_LIST_STORE(model), &iter);
    }
}

void on_propriedades_clicked(GtkButton *button, GtkTreeView *tree_view) {
    GtkTreeSelection *selection = gtk_tree_view_get_selection(tree_view);
    GtkTreeModel *model;
    GtkTreeIter iter;
    
    if (gtk_tree_selection_get_selected(selection, &model, &iter)) {
        gchar *nome;
        gint idade;
        gchar *cidade;
        gboolean ativo;
        
        gtk_tree_model_get(model, &iter,
                          COLUNA_NOME, &nome,
                          COLUNA_IDADE, &idade,
                          COLUNA_CIDADE, &cidade,
                          COLUNA_ATIVO, &ativo,
                          -1);
        
        GtkWidget *dialog = gtk_message_dialog_new(
            GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(tree_view))),
            GTK_DIALOG_DESTROY_WITH_PARENT,
            GTK_MESSAGE_INFO,
            GTK_BUTTONS_OK,
            "Nome: %s\nIdade: %d\nCidade: %s\nStatus: %s",
            nome, idade, cidade, ativo ? "Ativo" : "Inativo");
        
        gtk_dialog_run(GTK_DIALOG(dialog));
        gtk_widget_destroy(dialog);
        
        g_free(nome);
        g_free(cidade);
    }
}
```

### 7.2 TextView (Editor de Texto)

```c
#include <gtk/gtk.h>

// Estrutura para o editor
typedef struct {
    GtkWidget *text_view;
    GtkWidget *status_label;
    gchar *arquivo_atual;
} Editor;

// Aplicar tags de formatação
void aplicar_tag(GtkTextBuffer *buffer, const gchar *tag_name) {
    GtkTextIter start, end;
    
    if (gtk_text_buffer_get_selection_bounds(buffer, &start, &end)) {
        gtk_text_buffer_apply_tag_by_name(buffer, tag_name, &start, &end);
    }
}

// Callback para mudanças no buffer
void on_buffer_changed(GtkTextBuffer *buffer, Editor *editor) {
    gint chars = gtk_text_buffer_get_char_count(buffer);
    gint lines = gtk_text_buffer_get_line_count(buffer);
    
    gchar *status = g_strdup_printf("Caracteres: %d | Linhas: %d", chars, lines);
    gtk_label_set_text(GTK_LABEL(editor->status_label), status);
    g_free(status);
}

// Callback para salvar arquivo
void on_salvar_clicked(GtkButton *button, Editor *editor) {
    GtkTextBuffer *buffer = gtk_text_view_get_buffer(
        GTK_TEXT_VIEW(editor->text_view));
    
    GtkTextIter start, end;
    gtk_text_buffer_get_bounds(buffer, &start, &end);
    gchar *text = gtk_text_buffer_get_text(buffer, &start, &end, FALSE);
    
    // Salvar em arquivo
    if (editor->arquivo_atual) {
        g_file_set_contents(editor->arquivo_atual, text, -1, NULL);
        g_print("Arquivo salvo: %s\n", editor->arquivo_atual);
    } else {
        // Abrir diálogo para escolher local
        GtkWidget *dialog = gtk_file_chooser_dialog_new(
            "Salvar Arquivo",
            GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(button))),
            GTK_FILE_CHOOSER_ACTION_SAVE,
            "_Cancelar", GTK_RESPONSE_CANCEL,
            "_Salvar", GTK_RESPONSE_ACCEPT,
            NULL);
        
        if (gtk_dialog_run(GTK_DIALOG(dialog)) == GTK_RESPONSE_ACCEPT) {
            gchar *filename = gtk_file_chooser_get_filename(
                GTK_FILE_CHOOSER(dialog));
            g_file_set_contents(filename, text, -1, NULL);
            editor->arquivo_atual = g_strdup(filename);
            g_free(filename);
        }
        
        gtk_widget_destroy(dialog);
    }
    
    g_free(text);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    Editor *editor = g_new(Editor, 1);
    editor->arquivo_atual = NULL;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Editor de Texto");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 0);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // Barra de ferramentas
    GtkWidget *toolbar = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_set_border_width(GTK_CONTAINER(toolbar), 5);
    gtk_box_pack_start(GTK_BOX(vbox), toolbar, FALSE, FALSE, 0);
    
    // Botões de formatação
    GtkWidget *btn_negrito = gtk_button_new_with_label("B");
    GtkWidget *btn_italico = gtk_button_new_with_label("I");
    GtkWidget *btn_sublinhado = gtk_button_new_with_label("U");
    GtkWidget *btn_salvar = gtk_button_new_with_label("Salvar");
    GtkWidget *btn_abrir = gtk_button_new_with_label("Abrir");
    
    gtk_box_pack_start(GTK_BOX(toolbar), btn_negrito, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_italico, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_sublinhado, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), gtk_separator_new(GTK_ORIENTATION_VERTICAL), 
                      FALSE, FALSE, 5);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_salvar, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_abrir, FALSE, FALSE, 0);
    
    // Área de texto com scroll
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, GTK_POLICY_AUTOMATIC);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    editor->text_view = gtk_text_view_new();
    gtk_container_add(GTK_CONTAINER(scrolled), editor->text_view);
    
    // Configurar buffer com tags de formatação
    GtkTextBuffer *buffer = gtk_text_view_get_buffer(
        GTK_TEXT_VIEW(editor->text_view));
    
    // Criar tags de formatação
    gtk_text_buffer_create_tag(buffer, "negrito",
                               "weight", PANGO_WEIGHT_BOLD, NULL);
    gtk_text_buffer_create_tag(buffer, "italico",
                               "style", PANGO_STYLE_ITALIC, NULL);
    gtk_text_buffer_create_tag(buffer, "sublinhado",
                               "underline", PANGO_UNDERLINE_SINGLE, NULL);
    
    // Barra de status
    editor->status_label = gtk_label_new("Caracteres: 0 | Linhas: 1");
    gtk_box_pack_start(GTK_BOX(vbox), editor->status_label, FALSE, FALSE, 0);
    
    // Conectar sinais
    g_signal_connect(buffer, "changed", 
                     G_CALLBACK(on_buffer_changed), editor);
    g_signal_connect(btn_negrito, "clicked",
                     G_CALLBACK(aplicar_tag), buffer);
    g_signal_connect_swapped(btn_negrito, "clicked",
                             G_CALLBACK(aplicar_tag), buffer);
    g_signal_connect(btn_salvar, "clicked",
                     G_CALLBACK(on_salvar_clicked), editor);
    
    // Ajustar parâmetros dos callbacks de formatação
    g_signal_connect_swapped(btn_negrito, "clicked",
                             G_CALLBACK(aplicar_tag), buffer);
    // ... similar para outros botões
    
    gtk_widget_show_all(window);
    gtk_main();
    
    g_free(editor->arquivo_atual);
    g_free(editor);
    
    return 0;
}
```

---

## 8. Desenhando com Cairo {#cairo}

### 8.1 Área de Desenho Básica

```c
#include <gtk/gtk.h>
#include <math.h>

// Estrutura para dados do desenho
typedef struct {
    double x, y;        // Posição do mouse
    gboolean desenhando; // Flag para desenho
    double cor_r, cor_g, cor_b; // Cor atual
} DesenhoDados;

// Callback de desenho (Cairo)
gboolean on_draw(GtkWidget *widget, cairo_t *cr, gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    // Obter dimensões da área de desenho
    GtkAllocation allocation;
    gtk_widget_get_allocation(widget, &allocation);
    int width = allocation.width;
    int height = allocation.height;
    
    // Fundo branco
    cairo_set_source_rgb(cr, 1, 1, 1);
    cairo_paint(cr);
    
    // Desenhar grade
    cairo_set_source_rgb(cr, 0.9, 0.9, 0.9);
    cairo_set_line_width(cr, 0.5);
    
    for (int i = 0; i <= width; i += 20) {
        cairo_move_to(cr, i, 0);
        cairo_line_to(cr, i, height);
    }
    
    for (int i = 0; i <= height; i += 20) {
        cairo_move_to(cr, 0, i);
        cairo_line_to(cr, width, i);
    }
    cairo_stroke(cr);
    
    // Desenhar círculo central
    double center_x = width / 2.0;
    double center_y = height / 2.0;
    double radius = 100.0;
    
    // Gradiente radial
    cairo_pattern_t *grad = cairo_pattern_create_radial(
        center_x - 20, center_y - 20, 20,
        center_x, center_y, radius);
    
    cairo_pattern_add_color_stop_rgb(grad, 0, 1, 0, 0); // Vermelho no centro
    cairo_pattern_add_color_stop_rgb(grad, 0.5, 0, 1, 0); // Verde no meio
    cairo_pattern_add_color_stop_rgb(grad, 1, 0, 0, 1); // Azul na borda
    
    cairo_arc(cr, center_x, center_y, radius, 0, 2 * M_PI);
    cairo_set_source(cr, grad);
    cairo_fill_preserve(cr);
    
    cairo_pattern_destroy(grad);
    
    // Contorno preto
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 2);
    cairo_stroke(cr);
    
    // Desenhar posição do mouse se estiver desenhando
    if (dados->desenhando) {
        cairo_set_source_rgb(cr, dados->cor_r, dados->cor_g, dados->cor_b);
        cairo_arc(cr, dados->x, dados->y, 5, 0, 2 * M_PI);
        cairo_fill(cr);
    }
    
    return FALSE;
}

// Eventos do mouse
gboolean on_motion_notify(GtkWidget *widget, GdkEventMotion *event,
                          gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (dados->desenhando) {
        dados->x = event->x;
        dados->y = event->y;
        gtk_widget_queue_draw(widget); // Solicitar redesenho
    }
    
    return TRUE;
}

gboolean on_button_press(GtkWidget *widget, GdkEventButton *event,
                         gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (event->button == 1) { // Botão esquerdo
        dados->desenhando = TRUE;
        dados->x = event->x;
        dados->y = event->y;
        
        // Mudar cor aleatoriamente
        dados->cor_r = (double)rand() / RAND_MAX;
        dados->cor_g = (double)rand() / RAND_MAX;
        dados->cor_b = (double)rand() / RAND_MAX;
        
        gtk_widget_queue_draw(widget);
    }
    
    return TRUE;
}

gboolean on_button_release(GtkWidget *widget, GdkEventButton *event,
                           gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (event->button == 1) {
        dados->desenhando = FALSE;
        gtk_widget_queue_draw(widget);
    }
    
    return TRUE;
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Inicializar dados de desenho
    DesenhoDados dados;
    dados.x = 0;
    dados.y = 0;
    dados.desenhando = FALSE;
    dados.cor_r = 1;
    dados.cor_g = 0;
    dados.cor_b = 0;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Desenhando com Cairo");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Área de desenho
    GtkWidget *drawing_area = gtk_drawing_area_new();
    gtk_container_add(GTK_CONTAINER(window), drawing_area);
    
    // Habilitar eventos do mouse
    gtk_widget_add_events(drawing_area,
                          GDK_BUTTON_PRESS_MASK |
                          GDK_BUTTON_RELEASE_MASK |
                          GDK_POINTER_MOTION_MASK);
    
    // Conectar sinais
    g_signal_connect(drawing_area, "draw", 
                     G_CALLBACK(on_draw), &dados);
    g_signal_connect(drawing_area, "motion-notify-event",
                     G_CALLBACK(on_motion_notify), &dados);
    g_signal_connect(drawing_area, "button-press-event",
                     G_CALLBACK(on_button_press), &dados);
    g_signal_connect(drawing_area, "button-release-event",
                     G_CALLBACK(on_button_release), &dados);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

### 8.2 Gráficos e Animações

```c
#include <gtk/gtk.h>
#include <math.h>
#include <time.h>

typedef struct {
    double angulo;
    double escala;
    gboolean animando;
    guint timeout_id;
    double pontos[100][2]; // Pontos para o gráfico
    int num_pontos;
} AnimacaoDados;

// Gerar dados do gráfico (seno com ruído)
void gerar_dados(AnimacaoDados *dados) {
    dados->num_pontos = 100;
    for (int i = 0; i < dados->num_pontos; i++) {
        double x = (double)i / (dados->num_pontos - 1) * 2 * M_PI;
        dados->pontos[i][0] = x;
        dados->pontos[i][1] = sin(x) + (rand() % 100) / 500.0;
    }
}

// Callback de animação
gboolean on_timeout(gpointer user_data) {
    AnimacaoDados *dados = (AnimacaoDados *)user_data;
    
    if (dados->animando) {
        dados->angulo += 0.05;
        dados->escala = 0.8 + 0.2 * sin(dados->angulo * 0.5);
        gtk_widget_queue_draw(GTK_WIDGET(dados->drawing_area));
    }
    
    return G_SOURCE_CONTINUE;
}

// Desenhar gráfico
void desenhar_grafico(cairo_t *cr, AnimacaoDados *dados, 
                      int width, int height) {
    // Margens
    double left_margin = 50;
    double right_margin = 50;
    double top_margin = 30;
    double bottom_margin = 40;
    
    double graph_width = width - left_margin - right_margin;
    double graph_height = height - top_margin - bottom_margin;
    
    // Fundo branco para o gráfico
    cairo_set_source_rgb(cr, 1, 1, 1);
    cairo_rectangle(cr, left_margin, top_margin, 
                    graph_width, graph_height);
    cairo_fill(cr);
    
    // Borda do gráfico
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 1);
    cairo_rectangle(cr, left_margin, top_margin, 
                    graph_width, graph_height);
    cairo_stroke(cr);
    
    // Desenhar linhas de grade
    cairo_set_source_rgb(cr, 0.8, 0.8, 0.8);
    cairo_set_line_width(cr, 0.5);
    
    for (int i = 0; i <= 4; i++) {
        double x = left_margin + (i * graph_width / 4);
        cairo_move_to(cr, x, top_margin);
        cairo_line_to(cr, x, top_margin + graph_height);
        
        double y = top_margin + (i * graph_height / 4);
        cairo_move_to(cr, left_margin, y);
        cairo_line_to(cr, left_margin + graph_width, y);
    }
    cairo_stroke(cr);
    
    // Desenhar pontos do gráfico
    if (dados->num_pontos > 0) {
        cairo_set_source_rgb(cr, 0, 0, 1);
        cairo_set_line_width(cr, 2);
        
        for (int i = 0; i < dados->num_pontos - 1; i++) {
            double x1 = left_margin + 
                (dados->pontos[i][0] / (2 * M_PI)) * graph_width;
            double y1 = top_margin + graph_height - 
                ((dados->pontos[i][1] + 1) / 2) * graph_height;
            
            double x2 = left_margin + 
                (dados->pontos[i+1][0] / (2 * M_PI)) * graph_width;
            double y2 = top_margin + graph_height - 
                ((dados->pontos[i+1][1] + 1) / 2) * graph_height;
            
            cairo_move_to(cr, x1, y1);
            cairo_line_to(cr, x2, y2);
        }
        cairo_stroke(cr);
    }
    
    // Rótulos dos eixos
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_select_font_face(cr, "Sans", CAIRO_FONT_SLANT_NORMAL,
                           CAIRO_FONT_WEIGHT_NORMAL);
    cairo_set_font_size(cr, 12);
    
    cairo_move_to(cr, width / 2, height - 10);
    cairo_show_text(cr, "Tempo (s)");
    
    cairo_save(cr);
    cairo_translate(cr, 20, height / 2);
    cairo_rotate(cr, -M_PI / 2);
    cairo_show_text(cr, "Amplitude");
    cairo_restore(cr);
}

gboolean on_draw(GtkWidget *widget, cairo_t *cr, gpointer user_data) {
    AnimacaoDados *dados = (AnimacaoDados *)user_data;
    
    GtkAllocation allocation;
    gtk_widget_get_allocation(widget, &allocation);
    int width = allocation.width;
    int height = allocation.height;
    
    // Fundo
    cairo_set_source_rgb(cr, 0.95, 0.95, 0.95);
    cairo_paint(cr);
    
    // Desenhar gráfico
    desenhar_grafico(cr, dados, width, height);
    
    // Desenhar retângulo animado
    double rect_size = 50 * dados->escala;
    double rect_x = width / 2 - rect_size / 2;
    double rect_y = height / 4 - rect_size / 2;
    
    // Rotação
    cairo_save(cr);
    cairo_translate(cr, rect_x + rect_size / 2, rect_y + rect_size / 2);
    cairo_rotate(cr, dados->angulo);
    cairo_rectangle(cr, -rect_size / 2, -rect_size / 2, 
                    rect_size, rect_size);
    
    // Gradiente para o retângulo
    cairo_pattern_t *grad = cairo_pattern_create_linear(
        -rect_size / 2, -rect_size / 2,
        rect_size / 2, rect_size / 2);
    cairo_pattern_add_color_stop_rgb(grad, 0, 1, 0, 0);
    cairo_pattern_add_color_stop_rgb(grad, 1, 0, 0, 1);
    
    cairo_set_source(cr, grad);
    cairo_fill_preserve(cr);
    cairo_pattern_destroy(grad);
    
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 2);
    cairo_stroke(cr);
    
    cairo_restore(cr);
    
    return FALSE;
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    srand(time(NULL));
    
    AnimacaoDados dados;
    dados.angulo = 0;
    dados.escala = 1;
    dados.animando = TRUE;
    gerar_dados(&dados);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Gráficos e Animação");
    gtk_window_set_default_size(GTK_WINDOW(window), 800, 500);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // Área de desenho
    GtkWidget *drawing_area = gtk_drawing_area_new();
    gtk_box_pack_start(GTK_BOX(vbox), drawing_area, TRUE, TRUE, 0);
    
    dados.drawing_area = drawing_area;
    
    // Barra de controle
    GtkWidget *hbox = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_set_border_width(GTK_CONTAINER(hbox), 5);
    gtk_box_pack_start(GTK_BOX(vbox), hbox, FALSE, FALSE, 0);
    
    GtkWidget *btn_iniciar = gtk_button_new_with_label("Iniciar");
    GtkWidget *btn_parar = gtk_button_new_with_label("Parar");
    GtkWidget *btn_novos_dados = gtk_button_new_with_label("Novos Dados");
    
    gtk_box_pack_start(GTK_BOX(hbox), btn_iniciar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox), btn_parar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox), btn_novos_dados, TRUE, TRUE, 0);
    
    // Conectar sinais
    g_signal_connect(drawing_area, "draw", G_CALLBACK(on_draw), &dados);
    
    g_signal_connect(btn_iniciar, "clicked", 
                     G_CALLBACK(on_iniciar_animacao), &dados);
    g_signal_connect(btn_parar, "clicked", 
                     G_CALLBACK(on_parar_animacao), &dados);
    g_signal_connect(btn_novos_dados, "clicked", 
                     G_CALLBACK(on_novos_dados), &dados);
    
    // Iniciar timer para animação
    dados.timeout_id = g_timeout_add(50, on_timeout, &dados);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    // Limpar timer
    if (dados.timeout_id > 0) {
        g_source_remove(dados.timeout_id);
    }
    
    return 0;
}

void on_iniciar_animacao(GtkButton *button, AnimacaoDados *dados) {
    dados->animando = TRUE;
}

void on_parar_animacao(GtkButton *button, AnimacaoDados *dados) {
    dados->animando = FALSE;
}

void on_novos_dados(GtkButton *button, AnimacaoDados *dados) {
    gerar_dados(dados);
    gtk_widget_queue_draw(GTK_WIDGET(dados->drawing_area));
}
```

---

## 9. Projeto Prático {#projeto}

### Calculadora Completa com GTK

Vamos construir uma calculadora completa que demonstra todos os conceitos aprendidos:

```c
#include <gtk/gtk.h>
#include <math.h>
#include <string.h>
#include <stdlib.h>

// Estrutura da calculadora
typedef struct {
    GtkWidget *entry_display;
    GtkWidget *entry_memoria;
    double valor_atual;
    double memoria;
    char operacao_pendente;
    gboolean novo_numero;
    gboolean ponto_inserido;
    GtkWidget *botoes[20]; // Para referência
} Calculadora;

// Protótipos
void calculadora_adicionar_digito(Calculadora *calc, char digito);
void calculadora_adicionar_ponto(Calculadora *calc);
void calculadora_limpar(Calculadora *calc);
void calculadora_limpar_tudo(Calculadora *calc);
void calculadora_operacao(Calculadora *calc, char op);
void calculadora_igual(Calculadora *calc);
void calculadora_raiz(Calculadora *calc);
void calculadora_porcentagem(Calculadora *calc);
void calculadora_memoria_limpar(Calculadora *calc);
void calculadora_memoria_recall(Calculadora *calc);
void calculadora_memoria_somar(Calculadora *calc);
void calculadora_memoria_subtrair(Calculadora *calc);
void calculadora_atualizar_display(Calculadora *calc);
void calculadora_atualizar_memoria(Calculadora *calc);

// Callbacks
void on_digito_clicked(GtkButton *button, Calculadora *calc) {
    const char *label = gtk_button_get_label(button);
    calculadora_adicionar_digito(calc, label[0]);
}

void on_ponto_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_adicionar_ponto(calc);
}

void on_operacao_clicked(GtkButton *button, Calculadora *calc) {
    const char *label = gtk_button_get_label(button);
    calculadora_operacao(calc, label[0]);
}

void on_igual_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_igual(calc);
}

void on_limpar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_limpar(calc);
}

void on_limpar_tudo_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_limpar_tudo(calc);
}

void on_raiz_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_raiz(calc);
}

void on_porcentagem_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_porcentagem(calc);
}

void on_memoria_limpar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_limpar(calc);
}

void on_memoria_recall_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_recall(calc);
}

void on_memoria_somar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_somar(calc);
}

void on_memoria_subtrair_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_subtrair(calc);
}

// Implementação das funções da calculadora
void calculadora_adicionar_digito(Calculadora *calc, char digito) {
    const char *texto_atual = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    
    if (calc->novo_numero) {
        char novo_texto[2] = {digito, '\0'};
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
        calc->novo_numero = FALSE;
        calc->ponto_inserido = FALSE;
    } else {
        char novo_texto[256];
        snprintf(novo_texto, sizeof(novo_texto), "%s%c", texto_atual, digito);
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
    }
}

void calculadora_adicionar_ponto(Calculadora *calc) {
    if (!calc->ponto_inserido) {
        const char *texto_atual = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
        
        if (calc->novo_numero) {
            gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0.");
            calc->novo_numero = FALSE;
        } else {
            char novo_texto[256];
            snprintf(novo_texto, sizeof(novo_texto), "%s.", texto_atual);
            gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
        }
        calc->ponto_inserido = TRUE;
    }
}

void calculadora_limpar(Calculadora *calc) {
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0");
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
}

void calculadora_limpar_tudo(Calculadora *calc) {
    calc->valor_atual = 0;
    calc->operacao_pendente = '\0';
    calculadora_limpar(calc);
}

void calculadora_operacao(Calculadora *calc, char op) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    
    if (calc->operacao_pendente != '\0') {
        calculadora_igual(calc);
        calc->operacao_pendente = op;
    } else {
        calc->valor_atual = valor;
        calc->operacao_pendente = op;
        calc->novo_numero = TRUE;
    }
}

void calculadora_igual(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double segundo_valor = atof(texto);
    double resultado = 0;
    
    switch (calc->operacao_pendente) {
        case '+':
            resultado = calc->valor_atual + segundo_valor;
            break;
        case '-':
            resultado = calc->valor_atual - segundo_valor;
            break;
        case '*':
            resultado = calc->valor_atual * segundo_valor;
            break;
        case '/':
            if (segundo_valor != 0) {
                resultado = calc->valor_atual / segundo_valor;
            } else {
                gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "Erro");
                calc->novo_numero = TRUE;
                calc->operacao_pendente = '\0';
                return;
            }
            break;
        default:
            resultado = segundo_valor;
    }
    
    char resultado_texto[64];
    snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
    
    calc->valor_atual = resultado;
    calc->operacao_pendente = '\0';
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
}

void calculadora_raiz(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    
    if (valor >= 0) {
        double resultado = sqrt(valor);
        char resultado_texto[64];
        snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
        calc->novo_numero = TRUE;
    } else {
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "Erro");
        calc->novo_numero = TRUE;
    }
}

void calculadora_porcentagem(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    double resultado = valor / 100.0;
    
    char resultado_texto[64];
    snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
    calc->novo_numero = TRUE;
}

void calculadora_memoria_limpar(Calculadora *calc) {
    calc->memoria = 0;
    calculadora_atualizar_memoria(calc);
}

void calculadora_memoria_recall(Calculadora *calc) {
    char memoria_texto[64];
    snprintf(memoria_texto, sizeof(memoria_texto), "%g", calc->memoria);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), memoria_texto);
    calc->novo_numero = TRUE;
}

void calculadora_memoria_somar(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    calc->memoria += valor;
    calculadora_atualizar_memoria(calc);
}

void calculadora_memoria_subtrair(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    calc->memoria -= valor;
    calculadora_atualizar_memoria(calc);
}

void calculadora_atualizar_memoria(Calculadora *calc) {
    char memoria_texto[64];
    if (calc->memoria != 0) {
        snprintf(memoria_texto, sizeof(memoria_texto), "M: %g", calc->memoria);
    } else {
        snprintf(memoria_texto, sizeof(memoria_texto), "M: 0");
    }
    gtk_entry_set_text(GTK_ENTRY(calc->entry_memoria), memoria_texto);
}

// Função principal
int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Alocar e inicializar calculadora
    Calculadora *calc = g_new(Calculadora, 1);
    calc->valor_atual = 0;
    calc->memoria = 0;
    calc->operacao_pendente = '\0';
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
    
    // Criar janela
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Calculadora GTK");
    gtk_window_set_default_size(GTK_WINDOW(window), 350, 400);
    gtk_window_set_resizable(GTK_WINDOW(window), FALSE);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Container principal
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Display e memória
    calc->entry_display = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0");
    gtk_entry_set_alignment(GTK_ENTRY(calc->entry_display), 1);
    gtk_entry_set_max_length(GTK_ENTRY(calc->entry_display), 20);
    gtk_widget_set_size_request(calc->entry_display, -1, 50);
    gtk_box_pack_start(GTK_BOX(vbox), calc->entry_display, FALSE, FALSE, 0);
    
    calc->entry_memoria = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(calc->entry_memoria), "M: 0");
    gtk_entry_set_alignment(GTK_ENTRY(calc->entry_memoria), 1);
    gtk_editable_set_editable(GTK_EDITABLE(calc->entry_memoria), FALSE);
    gtk_widget_set_size_request(calc->entry_memoria, -1, 30);
    gtk_box_pack_start(GTK_BOX(vbox), calc->entry_memoria, FALSE, FALSE, 0);
    
    // Grid para botões
    GtkWidget *grid = gtk_grid_new();
    gtk_grid_set_row_spacing(GTK_GRID(grid), 5);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 5);
    gtk_box_pack_start(GTK_BOX(vbox), grid, TRUE, TRUE, 0);
    
    // Definir botões
    const char *botoes[6][5] = {
        {"MC", "MR", "M+", "M-", "C"},
        {"√", "%", "/", "*", "←"},
        {"7", "8", "9", "-", ""},
        {"4", "5", "6", "+", ""},
        {"1", "2", "3", "=", ""},
        {"0", ".", "", "", ""}
    };
    
    // Criar botões
    for (int i = 0; i < 6; i++) {
        for (int j = 0; j < 5; j++) {
            if (strlen(botoes[i][j]) > 0) {
                GtkWidget *button = gtk_button_new_with_label(botoes[i][j]);
                gtk_widget_set_size_request(button, 60, 50);
                gtk_grid_attach(GTK_GRID(grid), button, j, i, 1, 1);
                
                // Conectar sinais baseado no tipo de botão
                const char *label = botoes[i][j];
                
                if (isdigit(label[0])) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_digito_clicked), calc);
                } else if (strcmp(label, ".") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_ponto_clicked), calc);
                } else if (strcmp(label, "=") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_igual_clicked), calc);
                } else if (strcmp(label, "C") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_limpar_clicked), calc);
                } else if (strcmp(label, "←") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_limpar_clicked), calc);
                } else if (strcmp(label, "√") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_raiz_clicked), calc);
                } else if (strcmp(label, "%") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_porcentagem_clicked), calc);
                } else if (strcmp(label, "MC") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_limpar_clicked), calc);
                } else if (strcmp(label, "MR") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_recall_clicked), calc);
                } else if (strcmp(label, "M+") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_somar_clicked), calc);
                } else if (strcmp(label, "M-") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_subtrair_clicked), calc);
                } else if (strchr("+-*/", label[0])) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_operacao_clicked), calc);
                }
            }
        }
    }
    
    gtk_widget_show_all(window);
    gtk_main();
    
    g_free(calc);
    return 0;
}
```

---

## 10. Dicas e Boas Práticas {#dicas}

### 10.1 Gerenciamento de Memória

```c
// Sempre liberar memória alocada
gchar *texto = g_strdup_printf("Número: %d", 42);
// ... usar texto
g_free(texto);

// Para objetos GTK, usar g_object_unref quando apropriado
GtkWidget *widget = gtk_button_new();
// ... usar widget
g_object_unref(widget); // Se você tem uma referência extra

// Usar g_clear_pointer para liberar e anular
gchar *dados = g_strdup("importante");
g_clear_pointer(&dados, g_free);
```

### 10.2 Organização de Código

```c
// Separar em arquivos:
// main.c - ponto de entrada
// interface.c - criação da UI
// callbacks.c - funções de callback
// dados.h - estruturas e protótipos

// Exemplo de estrutura de projeto:
/*
projeto/
├── Makefile
├── src/
│   ├── main.c
│   ├── interface.c
│   ├── interface.h
│   ├── callbacks.c
│   ├── callbacks.h
│   └── dados.h
└── resources/
    ├── style.css
    └── icons/
*/
```

### 10.3 CSS para Estilização

```c
// Aplicar CSS personalizado
void aplicar_css(GtkWidget *widget) {
    GtkCssProvider *provider = gtk_css_provider_new();
    gtk_css_provider_load_from_data(provider,
        "button {"
        "   background-color: #3498db;"
        "   color: white;"
        "   border-radius: 5px;"
        "}"
        "button:hover {"
        "   background-color: #2980b9;"
        "}"
        "entry {"
        "   background-color: #ecf0f1;"
        "   border: 1px solid #bdc3c7;"
        "   border-radius: 3px;"
        "}", -1, NULL);
    
    GtkStyleContext *context = gtk_widget_get_style_context(widget);
    gtk_style_context_add_provider(context,
        GTK_STYLE_PROVIDER(provider),
        GTK_STYLE_PROVIDER_PRIORITY_USER);
    
    g_object_unref(provider);
}
```

### 10.4 Internacionalização

```c
#include <libintl.h>
#include <locale.h>

#define _(STRING) gettext(STRING)

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");
    bindtextdomain("meuapp", "/usr/share/locale");
    textdomain("meuapp");
    
    gtk_init(&argc, &argv);
    
    GtkWidget *label = gtk_label_new(_("Hello World"));
    // ...
}
```

### 10.5 Debug e Logging

```c
// Usar g_print para debug
g_print("Valor atual: %f\n", valor);

// Usar g_warning para avisos
g_warning("Operação inválida: %s", operacao);

// Usar g_error para erros fatais
g_error("Falha ao criar widget");

// Usar g_return_if_fail para pré-condições
void minha_funcao(GtkWidget *widget) {
    g_return_if_fail(GTK_IS_WIDGET(widget));
    // ...
}
```

### 10.6 Checklist de Boas Práticas

- [ ] Sempre verificar ponteiros NULL
- [ ] Usar macros de cast (GTK_WIDGET, GTK_WINDOW, etc.)
- [ ] Liberar memória alocada
- [ ] Separar UI da lógica de negócio
- [ ] Usar nomes descritivos para callbacks
- [ ] Documentar funções complexas
- [ ] Tratar erros adequadamente
- [ ] Testar em diferentes resoluções
- [ ] Considerar acessibilidade
- [ ] Seguir as convenções de nomenclatura do GTK

---

## Conclusão

Parabéns! Você agora tem um conhecimento abrangente de GTK em C. Este manual cobriu desde conceitos básicos até tópicos avançados como desenho com Cairo e construção de aplicações completas.

Lembre-se:
- **Pratique** cada exemplo
- **Experimente** modificar os códigos
- **Consulte** a documentação oficial do GTK
- **Participe** da comunidade GTK

Bons códigos e divirta-se criando interfaces incríveis! 🚀# Manual Completo de GTK em C
## Aprenda a Criar Interfaces Gráficas Profissionais

---

## Sumário
1. [Introdução ao GTK](#introdução)
2. [Configuração do Ambiente](#configuracao)
3. [Primeiro Programa: "Hello World"](#primeiro-programa)
4. [Widgets Básicos](#widgets-basicos)
5. [Gerenciamento de Layout](#layout)
6. [Sinais e Callbacks](#sinais)
7. [Widgets Avançados](#widgets-avancados)
8. [Desenhando com Cairo](#cairo)
9. [Projeto Prático](#projeto)
10. [Dicas e Boas Práticas](#dicas)

---

## 1. Introdução ao GTK {#introducao}

### O que é GTK?
GTK (GIMP Toolkit) é um kit de ferramentas multiplataforma para criar interfaces gráficas. Originalmente desenvolvido para o GIMP, hoje é usado em aplicativos como Inkscape, Gedit e muitos outros.

### Filosofia do GTK
- **Orientado a objetos** (mesmo sendo em C)
- **Widgets são objetos** que emitem sinais
- **Hierarquia de containers** para organização
- **Separação entre lógica e apresentação**

### Estrutura Básica de um Programa GTK
Todo programa GTK segue este fluxo:
1. Inicializar o GTK
2. Criar widgets
3. Conectar sinais
4. Mostrar tudo
5. Loop principal

---

## 2. Configuração do Ambiente {#configuracao}

### Instalação no Linux (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install libgtk-3-dev pkg-config
```

### Instalação no Linux (Fedora)
```bash
sudo dnf install gtk3-devel
```

### Instalação no Windows
1. Baixe MSYS2 do site oficial
2. No terminal MSYS2:
```bash
pacman -S mingw-w64-x86_64-gtk3
```

### Compilando Programas GTK
```bash
# Comando básico de compilação
gcc programa.c -o programa `pkg-config --cflags --libs gtk+-3.0`

# Com mais flags de warning
gcc -Wall -g programa.c -o programa `pkg-config --cflags --libs gtk+-3.0`
```

---

## 3. Primeiro Programa: "Hello World" {#primeiro-programa}

Vamos criar nosso primeiro programa GTK, analisando cada linha detalhadamente:

```c
#include <gtk/gtk.h>

// Função callback chamada quando clicam no botão
static void hello_world(GtkWidget *widget, gpointer data) {
    g_print("Hello World\n");
}

// Função callback para fechar a janela
static void destroy(GtkWidget *widget, gpointer data) {
    gtk_main_quit();
}

int main(int argc, char *argv[]) {
    GtkWidget *window;
    GtkWidget *button;
    
    // 1. Inicializa o GTK
    gtk_init(&argc, &argv);
    
    // 2. Cria a janela principal
    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    
    // 3. Configura propriedades da janela
    gtk_window_set_title(GTK_WINDOW(window), "Meu Primeiro Programa");
    gtk_window_set_default_size(GTK_WINDOW(window), 300, 200);
    gtk_container_set_border_width(GTK_CONTAINER(window), 10);
    
    // 4. Conecta o sinal de destroy
    g_signal_connect(window, "destroy", G_CALLBACK(destroy), NULL);
    
    // 5. Cria um botão
    button = gtk_button_new_with_label("Clique Aqui");
    
    // 6. Conecta o sinal do botão
    g_signal_connect(button, "clicked", G_CALLBACK(hello_world), NULL);
    
    // 7. Adiciona o botão à janela
    gtk_container_add(GTK_CONTAINER(window), button);
    
    // 8. Mostra todos os widgets
    gtk_widget_show_all(window);
    
    // 9. Inicia o loop principal
    gtk_main();
    
    return 0;
}
```

### Explicação Detalhada:

#### `#include <gtk/gtk.h>`
Inclui todas as funções e definições necessárias do GTK.

#### `gtk_init(&argc, &argv)`
- **Propósito**: Inicializa o GTK
- **Parâmetros**: 
  - `argc`: ponteiro para contagem de argumentos
  - `argv`: ponteiro para array de argumentos
- **O que faz**: Configura o ambiente, carrega temas, inicializa subsystems

#### `gtk_window_new(GTK_WINDOW_TOPLEVEL)`
- **Propósito**: Cria uma nova janela
- **Parâmetro**: Tipo de janela
  - `GTK_WINDOW_TOPLEVEL`: janela padrão com bordas
  - `GTK_WINDOW_POPUP`: janela sem decorações
- **Retorno**: GtkWidget* (ponteiro para o widget)

#### Macros de Casting
- `GTK_WINDOW(window)`: converte GtkWidget* para GtkWindow*
- `GTK_CONTAINER(window)`: converte para container
- **Por que usar**: GTK usa herança em C, essas macros fazem type checking seguro

#### `g_signal_connect()`
- **Propósito**: Conecta um sinal a uma função callback
- **Parâmetros**:
  1. Instância do objeto
  2. Nome do sinal (string)
  3. Função callback (com cast G_CALLBACK)
  4. Dados do usuário (pode ser NULL)

#### `gtk_main()`
- **Propósito**: Inicia o loop principal do GTK
- **Comportamento**: Fica em loop até `gtk_main_quit()` ser chamado
- **Processa**: Eventos do mouse, teclado, redesenho, etc.

---

## 4. Widgets Básicos {#widgets-basicos}

### 4.1 Botões (GtkButton)

```c
#include <gtk/gtk.h>

void on_button_clicked(GtkButton *button, gpointer user_data) {
    const gchar *text = gtk_button_get_label(button);
    g_print("Botão '%s' clicado!\n", text);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Botões");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    // Container vertical
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // 1. Botão simples com texto
    GtkWidget *btn1 = gtk_button_new_with_label("Botão Simples");
    g_signal_connect(btn1, "clicked", G_CALLBACK(on_button_clicked), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn1, TRUE, TRUE, 0);
    
    // 2. Botão com ícone
    GtkWidget *btn2 = gtk_button_new();
    GtkWidget *box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    GtkWidget *icon = gtk_image_new_from_icon_name("document-save", 
                                                    GTK_ICON_SIZE_BUTTON);
    GtkWidget *label = gtk_label_new("Salvar");
    
    gtk_box_pack_start(GTK_BOX(box), icon, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(box), label, FALSE, FALSE, 0);
    gtk_container_add(GTK_CONTAINER(btn2), box);
    
    g_signal_connect(btn2, "clicked", G_CALLBACK(on_button_clicked), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn2, TRUE, TRUE, 0);
    
    // 3. Botão de toggle (liga/desliga)
    GtkWidget *btn3 = gtk_toggle_button_new_with_label("Modo Edição");
    g_signal_connect(btn3, "toggled", G_CALLBACK(on_toggle_changed), NULL);
    gtk_box_pack_start(GTK_BOX(vbox), btn3, TRUE, TRUE, 0);
    
    // 4. Botão de rádio
    GSList *group = NULL;
    GtkWidget *radio1 = gtk_radio_button_new_with_label(group, "Opção 1");
    group = gtk_radio_button_get_group(GTK_RADIO_BUTTON(radio1));
    GtkWidget *radio2 = gtk_radio_button_new_with_label(group, "Opção 2");
    GtkWidget *radio3 = gtk_radio_button_new_with_label(group, "Opção 3");
    
    gtk_box_pack_start(GTK_BOX(vbox), radio1, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(vbox), radio2, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(vbox), radio3, TRUE, TRUE, 0);
    
    // 5. Botão com link
    GtkWidget *btn_link = gtk_link_button_new("https://www.gtk.org");
    gtk_box_pack_start(GTK_BOX(vbox), btn_link, TRUE, TRUE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}

void on_toggle_changed(GtkToggleButton *button, gpointer user_data) {
    if (gtk_toggle_button_get_active(button)) {
        g_print("Toggle ativado\n");
    } else {
        g_print("Toggle desativado\n");
    }
}
```

### 4.2 Rótulos (GtkLabel)

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Labels");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 20);
    
    // 1. Label simples
    GtkWidget *label1 = gtk_label_new("Texto simples");
    gtk_box_pack_start(GTK_BOX(vbox), label1, FALSE, FALSE, 0);
    
    // 2. Label com formatação (Pango Markup)
    GtkWidget *label2 = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(label2), 
        "<span foreground='blue' size='x-large'><b>Texto em negrito azul</b></span>");
    gtk_box_pack_start(GTK_BOX(vbox), label2, FALSE, FALSE, 0);
    
    // 3. Label com múltiplas linhas
    GtkWidget *label3 = gtk_label_new(
        "Esta é uma label com múltiplas linhas\n"
        "Ela quebra automaticamente o texto\n"
        "Usando \\n para novas linhas"
    );
    gtk_box_pack_start(GTK_BOX(vbox), label3, FALSE, FALSE, 0);
    
    // 4. Label com justificação
    GtkWidget *label4 = gtk_label_new(
        "Este texto é justificado. Quando a janela for redimensionada, "
        "o texto será quebrado automaticamente e alinhado conforme a "
        "configuração de justificação escolhida."
    );
    gtk_label_set_justify(GTK_LABEL(label4), GTK_JUSTIFY_CENTER);
    gtk_label_set_line_wrap(GTK_LABEL(label4), TRUE);
    gtk_label_set_max_width_chars(GTK_LABEL(label4), 40);
    gtk_box_pack_start(GTK_BOX(vbox), label4, FALSE, FALSE, 0);
    
    // 5. Label com selecionável (para copiar texto)
    GtkWidget *label5 = gtk_label_new("Este texto pode ser selecionado e copiado");
    gtk_label_set_selectable(GTK_LABEL(label5), TRUE);
    gtk_box_pack_start(GTK_BOX(vbox), label5, FALSE, FALSE, 0);
    
    // 6. Label com ângulo (texto rotacionado)
    GtkWidget *label6 = gtk_label_new("Texto Rotacionado");
    gtk_label_set_angle(GTK_LABEL(label6), 45);
    gtk_box_pack_start(GTK_BOX(vbox), label6, FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 4.3 Entradas de Texto (GtkEntry)

```c
#include <gtk/gtk.h>

void on_entry_activate(GtkEntry *entry, gpointer user_data) {
    const gchar *text = gtk_entry_get_text(entry);
    g_print("Texto inserido: %s\n", text);
}

void on_toggle_visibility(GtkToggleButton *button, gpointer user_data) {
    GtkEntry *entry = GTK_ENTRY(user_data);
    gboolean active = gtk_toggle_button_get_active(button);
    gtk_entry_set_visibility(entry, !active);  // !active porque é "Ocultar"
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Entry");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 20);
    
    // 1. Entry simples
    GtkWidget *entry1 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry1), "Digite algo...");
    gtk_box_pack_start(GTK_BOX(vbox), entry1, FALSE, FALSE, 0);
    g_signal_connect(entry1, "activate", G_CALLBACK(on_entry_activate), NULL);
    
    // 2. Entry com texto pré-definido
    GtkWidget *entry2 = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(entry2), "Texto inicial");
    gtk_box_pack_start(GTK_BOX(vbox), entry2, FALSE, FALSE, 0);
    
    // 3. Entry para senha
    GtkWidget *entry3 = gtk_entry_new();
    gtk_entry_set_visibility(GTK_ENTRY(entry3), FALSE);
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry3), "Senha");
    gtk_box_pack_start(GTK_BOX(vbox), entry3, FALSE, FALSE, 0);
    
    // Botão para mostrar/ocultar senha
    GtkWidget *check = gtk_check_button_new_with_label("Ocultar senha");
    gtk_toggle_button_set_active(GTK_TOGGLE_BUTTON(check), TRUE);
    g_signal_connect(check, "toggled", G_CALLBACK(on_toggle_visibility), entry3);
    gtk_box_pack_start(GTK_BOX(vbox), check, FALSE, FALSE, 0);
    
    // 4. Entry com limitação de caracteres
    GtkWidget *entry4 = gtk_entry_new();
    gtk_entry_set_max_length(GTK_ENTRY(entry4), 10);
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry4), "Máx 10 caracteres");
    gtk_box_pack_start(GTK_BOX(vbox), entry4, FALSE, FALSE, 0);
    
    // 5. Entry com ícone
    GtkWidget *entry5 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry5), "Buscar...");
    gtk_entry_set_icon_from_icon_name(GTK_ENTRY(entry5), 
                                       GTK_ENTRY_ICON_PRIMARY, 
                                       "system-search");
    gtk_box_pack_start(GTK_BOX(vbox), entry5, FALSE, FALSE, 0);
    
    // 6. Entry numérico
    GtkWidget *entry6 = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(entry6), "Números apenas");
    gtk_entry_set_input_purpose(GTK_ENTRY(entry6), GTK_INPUT_PURPOSE_DIGITS);
    gtk_box_pack_start(GTK_BOX(vbox), entry6, FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

---

## 5. Gerenciamento de Layout {#layout}

### 5.1 Box (GtkBox) - Layout Linear

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Box");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    // Container principal: vertical
    GtkWidget *main_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), main_vbox);
    gtk_container_set_border_width(GTK_CONTAINER(main_vbox), 10);
    
    // Título
    GtkWidget *title = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(title), 
        "<span size='xx-large' weight='bold'>Layout com Box</span>");
    gtk_box_pack_start(GTK_BOX(main_vbox), title, FALSE, FALSE, 10);
    
    // Linha horizontal 1: Botões alinhados à esquerda
    GtkWidget *hbox1 = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(main_vbox), hbox1, FALSE, FALSE, 0);
    
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 1"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 2"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(hbox1), 
        gtk_button_new_with_label("Esquerda 3"), FALSE, FALSE, 0);
    
    // Linha horizontal 2: Botões expandindo
    GtkWidget *hbox2 = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(main_vbox), hbox2, FALSE, FALSE, 0);
    
    GtkWidget *btn_expand1 = gtk_button_new_with_label("Expande");
    GtkWidget *btn_expand2 = gtk_button_new_with_label("Expande também");
    GtkWidget *btn_fixo = gtk_button_new_with_label("Fixo");
    
    gtk_box_pack_start(GTK_BOX(hbox2), btn_expand1, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox2), btn_expand2, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox2), btn_fixo, FALSE, FALSE, 0);
    
    // Separador visual
    gtk_box_pack_start(GTK_BOX(main_vbox), 
        gtk_separator_new(GTK_ORIENTATION_HORIZONTAL), FALSE, FALSE, 10);
    
    // Área central com duas colunas
    GtkWidget *center_hbox = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 20);
    gtk_box_pack_start(GTK_BOX(main_vbox), center_hbox, TRUE, TRUE, 0);
    
    // Coluna esquerda (vertical)
    GtkWidget *left_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(center_hbox), left_vbox, TRUE, TRUE, 0);
    
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_label_new("Coluna Esquerda"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_button_new_with_label("Botão 1"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_vbox), 
        gtk_button_new_with_label("Botão 2"), FALSE, FALSE, 0);
    
    // Coluna direita (vertical)
    GtkWidget *right_vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(center_hbox), right_vbox, TRUE, TRUE, 0);
    
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_label_new("Coluna Direita"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_button_new_with_label("Botão A"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(right_vbox), 
        gtk_button_new_with_label("Botão B"), FALSE, FALSE, 0);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 5.2 Grid (GtkGrid) - Layout em Tabela

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Grid");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 300);
    
    // Criando o grid
    GtkWidget *grid = gtk_grid_new();
    gtk_container_add(GTK_CONTAINER(window), grid);
    gtk_container_set_border_width(GTK_CONTAINER(grid), 10);
    gtk_grid_set_row_spacing(GTK_GRID(grid), 10);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 10);
    
    // Preenchendo o grid
    // Linha 0: Título em todas as colunas
    GtkWidget *title = gtk_label_new(NULL);
    gtk_label_set_markup(GTK_LABEL(title), 
        "<span size='large' weight='bold'>Formulário de Cadastro</span>");
    gtk_grid_attach(GTK_GRID(grid), title, 0, 0, 2, 1);
    
    // Linha 1: Campos
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Nome:"), 0, 1, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 1, 1, 1);
    
    // Linha 2: Email
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Email:"), 0, 2, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 2, 1, 1);
    
    // Linha 3: Telefone
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_label_new("Telefone:"), 0, 3, 1, 1);
    gtk_grid_attach(GTK_GRID(grid), 
        gtk_entry_new(), 1, 3, 1, 1);
    
    // Linha 4: Sexo (usando duas colunas)
    GtkWidget *sexo_label = gtk_label_new("Sexo:");
    gtk_grid_attach(GTK_GRID(grid), sexo_label, 0, 4, 1, 1);
    
    GtkWidget *sexo_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Masculino"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Feminino"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(sexo_box), 
        gtk_radio_button_new_with_label(NULL, "Outro"), FALSE, FALSE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), sexo_box, 1, 4, 1, 1);
    
    // Linha 5: Interesses (checkboxes)
    GtkWidget *interesses_label = gtk_label_new("Interesses:");
    gtk_grid_attach(GTK_GRID(grid), interesses_label, 0, 5, 1, 1);
    
    GtkWidget *interesses_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Programação"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Design"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(interesses_box), 
        gtk_check_button_new_with_label("Música"), FALSE, FALSE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), interesses_box, 1, 5, 1, 1);
    
    // Linha 6: Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 10);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Salvar"), TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Cancelar"), TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), 
        gtk_button_new_with_label("Limpar"), TRUE, TRUE, 0);
    
    gtk_grid_attach(GTK_GRID(grid), button_box, 0, 6, 2, 1);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

### 5.3 Paned (GtkPaned) - Divisores Redimensionáveis

```c
#include <gtk/gtk.h>

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Layout com Paned");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    
    // Paned vertical (divide a janela em cima e baixo)
    GtkWidget *vpaned = gtk_paned_new(GTK_ORIENTATION_VERTICAL);
    gtk_container_add(GTK_CONTAINER(window), vpaned);
    
    // Parte superior - Paned horizontal
    GtkWidget *hpaned = gtk_paned_new(GTK_ORIENTATION_HORIZONTAL);
    gtk_paned_add1(GTK_PANED(vpaned), hpaned);
    
    // Lado esquerdo do paned horizontal
    GtkWidget *left_frame = gtk_frame_new("Navegação");
    gtk_frame_set_shadow_type(GTK_FRAME(left_frame), GTK_SHADOW_ETCHED_OUT);
    gtk_paned_add1(GTK_PANED(hpaned), left_frame);
    
    GtkWidget *left_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(left_frame), left_box);
    gtk_container_set_border_width(GTK_CONTAINER(left_box), 10);
    
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Página Inicial"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Arquivos"), FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(left_box), 
        gtk_button_new_with_label("Configurações"), FALSE, FALSE, 0);
    
    // Lado direito do paned horizontal
    GtkWidget *right_frame = gtk_frame_new("Conteúdo");
    gtk_paned_add2(GTK_PANED(hpaned), right_frame);
    
    GtkWidget *right_box = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(right_frame), right_box);
    gtk_container_set_border_width(GTK_CONTAINER(right_box), 10);
    
    gtk_box_pack_start(GTK_BOX(right_box), 
        gtk_label_new("Bem-vindo ao aplicativo!"), FALSE, FALSE, 0);
    
    // Parte inferior do paned vertical
    GtkWidget *bottom_frame = gtk_frame_new("Status");
    gtk_paned_add2(GTK_PANED(vpaned), bottom_frame);
    
    GtkWidget *status_label = gtk_label_new("Pronto");
    gtk_container_add(GTK_CONTAINER(bottom_frame), status_label);
    gtk_container_set_border_width(GTK_CONTAINER(bottom_frame), 10);
    
    // Definir posições iniciais
    gtk_paned_set_position(GTK_PANED(hpaned), 200);
    gtk_paned_set_position(GTK_PANED(vpaned), 300);
    
    gtk_widget_show_all(window);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_main();
    return 0;
}
```

---

## 6. Sinais e Callbacks {#sinais}

### 6.1 Tipos de Sinais Comuns

```c
#include <gtk/gtk.h>
#include <stdio.h>
#include <string.h>

// Estrutura para passar múltiplos dados
typedef struct {
    GtkWidget *entry;
    GtkWidget *label;
    GtkWidget *text_view;
} AppWidgets;

// Callback para clique do botão
void on_button_clicked(GtkButton *button, AppWidgets *widgets) {
    const gchar *text = gtk_entry_get_text(GTK_ENTRY(widgets->entry));
    
    if (strlen(text) > 0) {
        char markup[256];
        snprintf(markup, sizeof(markup), 
            "<span foreground='green' size='large'>%s</span>", text);
        gtk_label_set_markup(GTK_LABEL(widgets->label), markup);
        
        // Adicionar ao TextView
        GtkTextBuffer *buffer = gtk_text_view_get_buffer(
            GTK_TEXT_VIEW(widgets->text_view));
        GtkTextIter end;
        gtk_text_buffer_get_end_iter(buffer, &end);
        gtk_text_buffer_insert(buffer, &end, text, -1);
        gtk_text_buffer_insert(buffer, &end, "\n", -1);
    }
}

// Callback para mudança no entry
void on_entry_changed(GtkEditable *editable, AppWidgets *widgets) {
    const gchar *text = gtk_entry_get_text(GTK_ENTRY(editable));
    g_print("Texto mudou: %s\n", text);
}

// Callback para foco
gboolean on_entry_focus_in(GtkWidget *widget, GdkEventFocus *event, 
                           gpointer user_data) {
    g_print("Entry ganhou foco\n");
    gtk_widget_override_background_color(widget, GTK_STATE_FLAG_NORMAL,
        &(GdkRGBA){0.9, 0.9, 1.0, 1.0});
    return FALSE;
}

gboolean on_entry_focus_out(GtkWidget *widget, GdkEventFocus *event, 
                            gpointer user_data) {
    g_print("Entry perdeu foco\n");
    gtk_widget_override_background_color(widget, GTK_STATE_FLAG_NORMAL,
        &(GdkRGBA){1.0, 1.0, 1.0, 1.0});
    return FALSE;
}

// Callback para tecla pressionada
gboolean on_key_press(GtkWidget *widget, GdkEventKey *event, 
                      gpointer user_data) {
    g_print("Tecla pressionada: %s (código: %d)\n", 
            gdk_keyval_name(event->keyval), event->keyval);
    
    if (event->keyval == GDK_KEY_Escape) {
        gtk_main_quit();
        return TRUE;
    }
    return FALSE;
}

// Callback para quando a janela é fechada
gboolean on_window_delete_event(GtkWidget *widget, GdkEvent *event, 
                                gpointer user_data) {
    g_print("Janela sendo fechada...\n");
    // Retornar FALSE permite o fechamento, TRUE cancela
    return FALSE;
}

// Callback para redimensionamento da janela
void on_window_size_allocate(GtkWidget *widget, GdkRectangle *allocation,
                             gpointer user_data) {
    g_print("Janela redimensionada: %dx%d\n", 
            allocation->width, allocation->height);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    AppWidgets widgets;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo de Sinais");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 400);
    
    // Conectar sinais da janela
    g_signal_connect(window, "delete-event", 
                     G_CALLBACK(on_window_delete_event), NULL);
    g_signal_connect(window, "destroy", 
                     G_CALLBACK(gtk_main_quit), NULL);
    g_signal_connect(window, "size-allocate", 
                     G_CALLBACK(on_window_size_allocate), NULL);
    g_signal_connect(window, "key-press-event", 
                     G_CALLBACK(on_key_press), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 10);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Criar widgets
    widgets.entry = gtk_entry_new();
    gtk_entry_set_placeholder_text(GTK_ENTRY(widgets.entry), 
                                   "Digite algo...");
    gtk_box_pack_start(GTK_BOX(vbox), widgets.entry, FALSE, FALSE, 0);
    
    // Conectar sinais do entry
    g_signal_connect(widgets.entry, "changed", 
                     G_CALLBACK(on_entry_changed), &widgets);
    g_signal_connect(widgets.entry, "focus-in-event", 
                     G_CALLBACK(on_entry_focus_in), NULL);
    g_signal_connect(widgets.entry, "focus-out-event", 
                     G_CALLBACK(on_entry_focus_out), NULL);
    
    // Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_box_pack_start(GTK_BOX(vbox), button_box, FALSE, FALSE, 0);
    
    GtkWidget *btn_ok = gtk_button_new_with_label("OK");
    gtk_box_pack_start(GTK_BOX(button_box), btn_ok, TRUE, TRUE, 0);
    
    GtkWidget *btn_clear = gtk_button_new_with_label("Limpar");
    gtk_box_pack_start(GTK_BOX(button_box), btn_clear, TRUE, TRUE, 0);
    
    // Label para mostrar resultado
    widgets.label = gtk_label_new(NULL);
    gtk_box_pack_start(GTK_BOX(vbox), widgets.label, FALSE, FALSE, 0);
    
    // TextView para log
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, GTK_POLICY_AUTOMATIC);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    widgets.text_view = gtk_text_view_new();
    gtk_text_view_set_editable(GTK_TEXT_VIEW(widgets.text_view), FALSE);
    gtk_container_add(GTK_CONTAINER(scrolled), widgets.text_view);
    
    // Conectar botões
    g_signal_connect(btn_ok, "clicked", 
                     G_CALLBACK(on_button_clicked), &widgets);
    g_signal_connect_swapped(btn_clear, "clicked", 
                             G_CALLBACK(gtk_entry_set_text), 
                             widgets.entry);
    g_signal_connect_swapped(btn_clear, "clicked", 
                             G_CALLBACK(gtk_label_set_text), 
                             widgets.label);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

### 6.2 Trabalhando com Múltiplos Dados

```c
#include <gtk/gtk.h>
#include <stdlib.h>

// Estrutura complexa para dados do usuário
typedef struct {
    gchar *nome;
    gint idade;
    gchar *email;
    GtkWidget *entry_nome;
    GtkWidget *spin_idade;
    GtkWidget *entry_email;
} Usuario;

// Função para liberar memória
void usuario_free(Usuario *user) {
    if (user) {
        g_free(user->nome);
        g_free(user->email);
        g_free(user);
    }
}

// Callback para salvar dados
void on_salvar_clicked(GtkButton *button, Usuario *user) {
    // Atualizar dados da estrutura
    if (user->nome) g_free(user->nome);
    user->nome = g_strdup(gtk_entry_get_text(GTK_ENTRY(user->entry_nome)));
    
    user->idade = gtk_spin_button_get_value_as_int(
        GTK_SPIN_BUTTON(user->spin_idade));
    
    if (user->email) g_free(user->email);
    user->email = g_strdup(gtk_entry_get_text(GTK_ENTRY(user->entry_email)));
    
    g_print("Usuário salvo:\n");
    g_print("  Nome: %s\n", user->nome);
    g_print("  Idade: %d\n", user->idade);
    g_print("  Email: %s\n", user->email);
}

// Callback para mostrar dados em dialog
void on_mostrar_clicked(GtkButton *button, Usuario *user) {
    GtkWidget *dialog;
    gchar *message;
    
    message = g_strdup_printf(
        "<b>Dados do Usuário:</b>\n\n"
        "Nome: %s\n"
        "Idade: %d\n"
        "Email: %s",
        user->nome ? user->nome : "(não definido)",
        user->idade,
        user->email ? user->email : "(não definido)");
    
    dialog = gtk_message_dialog_new(
        GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(button))),
        GTK_DIALOG_DESTROY_WITH_PARENT,
        GTK_MESSAGE_INFO,
        GTK_BUTTONS_OK,
        "%s", message);
    
    gtk_message_dialog_set_markup(GTK_MESSAGE_DIALOG(dialog), message);
    
    g_signal_connect(dialog, "response", 
                     G_CALLBACK(gtk_widget_destroy), NULL);
    
    gtk_widget_show_all(dialog);
    g_free(message);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Alocar e inicializar estrutura do usuário
    Usuario *user = g_new0(Usuario, 1);
    user->nome = g_strdup("João Silva");
    user->idade = 25;
    user->email = g_strdup("joao@email.com");
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Dados do Usuário");
    gtk_window_set_default_size(GTK_WINDOW(window), 400, 250);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Quando a janela for destruída, liberar memória do usuário
    g_signal_connect_swapped(window, "destroy", 
                             G_CALLBACK(usuario_free), user);
    
    GtkWidget *grid = gtk_grid_new();
    gtk_container_add(GTK_CONTAINER(window), grid);
    gtk_container_set_border_width(GTK_CONTAINER(grid), 10);
    gtk_grid_set_row_spacing(GTK_GRID(grid), 5);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 5);
    
    // Criar e armazenar referências aos widgets
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Nome:"), 0, 0, 1, 1);
    user->entry_nome = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(user->entry_nome), user->nome);
    gtk_grid_attach(GTK_GRID(grid), user->entry_nome, 1, 0, 1, 1);
    
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Idade:"), 0, 1, 1, 1);
    user->spin_idade = gtk_spin_button_new_with_range(0, 120, 1);
    gtk_spin_button_set_value(GTK_SPIN_BUTTON(user->spin_idade), user->idade);
    gtk_grid_attach(GTK_GRID(grid), user->spin_idade, 1, 1, 1, 1);
    
    gtk_grid_attach(GTK_GRID(grid), gtk_label_new("Email:"), 0, 2, 1, 1);
    user->entry_email = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(user->entry_email), user->email);
    gtk_grid_attach(GTK_GRID(grid), user->entry_email, 1, 2, 1, 1);
    
    // Botões
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_grid_attach(GTK_GRID(grid), button_box, 0, 3, 2, 1);
    
    GtkWidget *btn_salvar = gtk_button_new_with_label("Salvar");
    GtkWidget *btn_mostrar = gtk_button_new_with_label("Mostrar");
    GtkWidget *btn_sair = gtk_button_new_with_label("Sair");
    
    gtk_box_pack_start(GTK_BOX(button_box), btn_salvar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_mostrar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_sair, TRUE, TRUE, 0);
    
    // Conectar sinais passando a estrutura do usuário
    g_signal_connect(btn_salvar, "clicked", G_CALLBACK(on_salvar_clicked), user);
    g_signal_connect(btn_mostrar, "clicked", G_CALLBACK(on_mostrar_clicked), user);
    g_signal_connect(btn_sair, "clicked", G_CALLBACK(gtk_main_quit), NULL);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

---

## 7. Widgets Avançados {#widgets-avancados}

### 7.1 TreeView (Listas e Árvores)

```c
#include <gtk/gtk.h>

// Estrutura para dados da lista
enum {
    COLUNA_NOME,
    COLUNA_IDADE,
    COLUNA_CIDADE,
    COLUNA_ATIVO,
    NUM_COLUNAS
};

// Função para adicionar uma pessoa à lista
void adicionar_pessoa(GtkListStore *store, const gchar *nome, 
                      gint idade, const gchar *cidade, gboolean ativo) {
    GtkTreeIter iter;
    gtk_list_store_append(store, &iter);
    gtk_list_store_set(store, &iter,
                       COLUNA_NOME, nome,
                       COLUNA_IDADE, idade,
                       COLUNA_CIDADE, cidade,
                       COLUNA_ATIVO, ativo,
                       -1);
}

// Callback para clique na árvore
void on_tree_row_activated(GtkTreeView *tree_view, GtkTreePath *path,
                           GtkTreeViewColumn *column, gpointer user_data) {
    GtkTreeModel *model = gtk_tree_view_get_model(tree_view);
    GtkTreeIter iter;
    
    if (gtk_tree_model_get_iter(model, &iter, path)) {
        gchar *nome;
        gint idade;
        gchar *cidade;
        gboolean ativo;
        
        gtk_tree_model_get(model, &iter,
                          COLUNA_NOME, &nome,
                          COLUNA_IDADE, &idade,
                          COLUNA_CIDADE, &cidade,
                          COLUNA_ATIVO, &ativo,
                          -1);
        
        g_print("Selecionado: %s, %d anos, %s, %s\n",
                nome, idade, cidade, ativo ? "Ativo" : "Inativo");
        
        g_free(nome);
        g_free(cidade);
    }
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Exemplo TreeView");
    gtk_window_set_default_size(GTK_WINDOW(window), 500, 300);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Criar o modelo de dados (ListStore)
    GtkListStore *store = gtk_list_store_new(NUM_COLUNAS,
                                             G_TYPE_STRING,
                                             G_TYPE_INT,
                                             G_TYPE_STRING,
                                             G_TYPE_BOOLEAN);
    
    // Adicionar dados
    adicionar_pessoa(store, "João Silva", 30, "São Paulo", TRUE);
    adicionar_pessoa(store, "Maria Santos", 25, "Rio de Janeiro", TRUE);
    adicionar_pessoa(store, "Pedro Oliveira", 35, "Belo Horizonte", FALSE);
    adicionar_pessoa(store, "Ana Costa", 28, "Curitiba", TRUE);
    adicionar_pessoa(store, "Carlos Souza", 42, "Porto Alegre", FALSE);
    
    // Criar a TreeView
    GtkWidget *tree_view = gtk_tree_view_new_with_model(GTK_TREE_MODEL(store));
    g_object_unref(store); // A tree_view agora tem sua própria referência
    
    // Configurar colunas
    // Coluna Nome
    GtkCellRenderer *renderer = gtk_cell_renderer_text_new();
    GtkTreeViewColumn *column = gtk_tree_view_column_new_with_attributes(
        "Nome", renderer, "text", COLUNA_NOME, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Idade
    renderer = gtk_cell_renderer_text_new();
    column = gtk_tree_view_column_new_with_attributes(
        "Idade", renderer, "text", COLUNA_IDADE, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Cidade
    renderer = gtk_cell_renderer_text_new();
    column = gtk_tree_view_column_new_with_attributes(
        "Cidade", renderer, "text", COLUNA_CIDADE, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Coluna Ativo (com toggle)
    renderer = gtk_cell_renderer_toggle_new();
    g_signal_connect(renderer, "toggled", 
                     G_CALLBACK(on_cell_toggled), store);
    column = gtk_tree_view_column_new_with_attributes(
        "Ativo", renderer, "active", COLUNA_ATIVO, NULL);
    gtk_tree_view_append_column(GTK_TREE_VIEW(tree_view), column);
    
    // Conectar sinal de ativação de linha
    g_signal_connect(tree_view, "row-activated",
                     G_CALLBACK(on_tree_row_activated), NULL);
    
    // Adicionar à janela com scroll
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, 
                                   GTK_POLICY_AUTOMATIC);
    gtk_container_add(GTK_CONTAINER(scrolled), tree_view);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    // Botões de controle
    GtkWidget *button_box = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_box_pack_start(GTK_BOX(vbox), button_box, FALSE, FALSE, 0);
    
    GtkWidget *btn_adicionar = gtk_button_new_with_label("Adicionar");
    GtkWidget *btn_remover = gtk_button_new_with_label("Remover");
    GtkWidget *btn_propriedades = gtk_button_new_with_label("Propriedades");
    
    gtk_box_pack_start(GTK_BOX(button_box), btn_adicionar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_remover, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(button_box), btn_propriedades, TRUE, TRUE, 0);
    
    // Conectar botões
    g_signal_connect(btn_adicionar, "clicked",
                     G_CALLBACK(on_adicionar_clicked), store);
    g_signal_connect(btn_remover, "clicked",
                     G_CALLBACK(on_remover_clicked), tree_view);
    g_signal_connect(btn_propriedades, "clicked",
                     G_CALLBACK(on_propriedades_clicked), tree_view);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}

// Callbacks para os botões
void on_cell_toggled(GtkCellRendererToggle *cell, gchar *path_str,
                     gpointer user_data) {
    GtkListStore *store = GTK_LIST_STORE(user_data);
    GtkTreeIter iter;
    GtkTreePath *path = gtk_tree_path_new_from_string(path_str);
    
    if (gtk_tree_model_get_iter(GTK_TREE_MODEL(store), &iter, path)) {
        gboolean ativo;
        gtk_tree_model_get(GTK_TREE_MODEL(store), &iter,
                          COLUNA_ATIVO, &ativo, -1);
        ativo = !ativo;
        gtk_list_store_set(store, &iter, COLUNA_ATIVO, ativo, -1);
    }
    
    gtk_tree_path_free(path);
}

void on_adicionar_clicked(GtkButton *button, GtkListStore *store) {
    // Simular adição de uma pessoa
    static int contador = 1;
    gchar *nome = g_strdup_printf("Novo Usuário %d", contador++);
    adicionar_pessoa(store, nome, 20 + (contador % 30), 
                     "Nova Cidade", TRUE);
    g_free(nome);
}

void on_remover_clicked(GtkButton *button, GtkTreeView *tree_view) {
    GtkTreeSelection *selection = gtk_tree_view_get_selection(tree_view);
    GtkTreeModel *model;
    GtkTreeIter iter;
    
    if (gtk_tree_selection_get_selected(selection, &model, &iter)) {
        gtk_list_store_remove(GTK_LIST_STORE(model), &iter);
    }
}

void on_propriedades_clicked(GtkButton *button, GtkTreeView *tree_view) {
    GtkTreeSelection *selection = gtk_tree_view_get_selection(tree_view);
    GtkTreeModel *model;
    GtkTreeIter iter;
    
    if (gtk_tree_selection_get_selected(selection, &model, &iter)) {
        gchar *nome;
        gint idade;
        gchar *cidade;
        gboolean ativo;
        
        gtk_tree_model_get(model, &iter,
                          COLUNA_NOME, &nome,
                          COLUNA_IDADE, &idade,
                          COLUNA_CIDADE, &cidade,
                          COLUNA_ATIVO, &ativo,
                          -1);
        
        GtkWidget *dialog = gtk_message_dialog_new(
            GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(tree_view))),
            GTK_DIALOG_DESTROY_WITH_PARENT,
            GTK_MESSAGE_INFO,
            GTK_BUTTONS_OK,
            "Nome: %s\nIdade: %d\nCidade: %s\nStatus: %s",
            nome, idade, cidade, ativo ? "Ativo" : "Inativo");
        
        gtk_dialog_run(GTK_DIALOG(dialog));
        gtk_widget_destroy(dialog);
        
        g_free(nome);
        g_free(cidade);
    }
}
```

### 7.2 TextView (Editor de Texto)

```c
#include <gtk/gtk.h>

// Estrutura para o editor
typedef struct {
    GtkWidget *text_view;
    GtkWidget *status_label;
    gchar *arquivo_atual;
} Editor;

// Aplicar tags de formatação
void aplicar_tag(GtkTextBuffer *buffer, const gchar *tag_name) {
    GtkTextIter start, end;
    
    if (gtk_text_buffer_get_selection_bounds(buffer, &start, &end)) {
        gtk_text_buffer_apply_tag_by_name(buffer, tag_name, &start, &end);
    }
}

// Callback para mudanças no buffer
void on_buffer_changed(GtkTextBuffer *buffer, Editor *editor) {
    gint chars = gtk_text_buffer_get_char_count(buffer);
    gint lines = gtk_text_buffer_get_line_count(buffer);
    
    gchar *status = g_strdup_printf("Caracteres: %d | Linhas: %d", chars, lines);
    gtk_label_set_text(GTK_LABEL(editor->status_label), status);
    g_free(status);
}

// Callback para salvar arquivo
void on_salvar_clicked(GtkButton *button, Editor *editor) {
    GtkTextBuffer *buffer = gtk_text_view_get_buffer(
        GTK_TEXT_VIEW(editor->text_view));
    
    GtkTextIter start, end;
    gtk_text_buffer_get_bounds(buffer, &start, &end);
    gchar *text = gtk_text_buffer_get_text(buffer, &start, &end, FALSE);
    
    // Salvar em arquivo
    if (editor->arquivo_atual) {
        g_file_set_contents(editor->arquivo_atual, text, -1, NULL);
        g_print("Arquivo salvo: %s\n", editor->arquivo_atual);
    } else {
        // Abrir diálogo para escolher local
        GtkWidget *dialog = gtk_file_chooser_dialog_new(
            "Salvar Arquivo",
            GTK_WINDOW(gtk_widget_get_toplevel(GTK_WIDGET(button))),
            GTK_FILE_CHOOSER_ACTION_SAVE,
            "_Cancelar", GTK_RESPONSE_CANCEL,
            "_Salvar", GTK_RESPONSE_ACCEPT,
            NULL);
        
        if (gtk_dialog_run(GTK_DIALOG(dialog)) == GTK_RESPONSE_ACCEPT) {
            gchar *filename = gtk_file_chooser_get_filename(
                GTK_FILE_CHOOSER(dialog));
            g_file_set_contents(filename, text, -1, NULL);
            editor->arquivo_atual = g_strdup(filename);
            g_free(filename);
        }
        
        gtk_widget_destroy(dialog);
    }
    
    g_free(text);
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    Editor *editor = g_new(Editor, 1);
    editor->arquivo_atual = NULL;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Editor de Texto");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 0);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // Barra de ferramentas
    GtkWidget *toolbar = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_set_border_width(GTK_CONTAINER(toolbar), 5);
    gtk_box_pack_start(GTK_BOX(vbox), toolbar, FALSE, FALSE, 0);
    
    // Botões de formatação
    GtkWidget *btn_negrito = gtk_button_new_with_label("B");
    GtkWidget *btn_italico = gtk_button_new_with_label("I");
    GtkWidget *btn_sublinhado = gtk_button_new_with_label("U");
    GtkWidget *btn_salvar = gtk_button_new_with_label("Salvar");
    GtkWidget *btn_abrir = gtk_button_new_with_label("Abrir");
    
    gtk_box_pack_start(GTK_BOX(toolbar), btn_negrito, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_italico, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_sublinhado, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), gtk_separator_new(GTK_ORIENTATION_VERTICAL), 
                      FALSE, FALSE, 5);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_salvar, FALSE, FALSE, 0);
    gtk_box_pack_start(GTK_BOX(toolbar), btn_abrir, FALSE, FALSE, 0);
    
    // Área de texto com scroll
    GtkWidget *scrolled = gtk_scrolled_window_new(NULL, NULL);
    gtk_scrolled_window_set_policy(GTK_SCROLLED_WINDOW(scrolled),
                                   GTK_POLICY_AUTOMATIC, GTK_POLICY_AUTOMATIC);
    gtk_box_pack_start(GTK_BOX(vbox), scrolled, TRUE, TRUE, 0);
    
    editor->text_view = gtk_text_view_new();
    gtk_container_add(GTK_CONTAINER(scrolled), editor->text_view);
    
    // Configurar buffer com tags de formatação
    GtkTextBuffer *buffer = gtk_text_view_get_buffer(
        GTK_TEXT_VIEW(editor->text_view));
    
    // Criar tags de formatação
    gtk_text_buffer_create_tag(buffer, "negrito",
                               "weight", PANGO_WEIGHT_BOLD, NULL);
    gtk_text_buffer_create_tag(buffer, "italico",
                               "style", PANGO_STYLE_ITALIC, NULL);
    gtk_text_buffer_create_tag(buffer, "sublinhado",
                               "underline", PANGO_UNDERLINE_SINGLE, NULL);
    
    // Barra de status
    editor->status_label = gtk_label_new("Caracteres: 0 | Linhas: 1");
    gtk_box_pack_start(GTK_BOX(vbox), editor->status_label, FALSE, FALSE, 0);
    
    // Conectar sinais
    g_signal_connect(buffer, "changed", 
                     G_CALLBACK(on_buffer_changed), editor);
    g_signal_connect(btn_negrito, "clicked",
                     G_CALLBACK(aplicar_tag), buffer);
    g_signal_connect_swapped(btn_negrito, "clicked",
                             G_CALLBACK(aplicar_tag), buffer);
    g_signal_connect(btn_salvar, "clicked",
                     G_CALLBACK(on_salvar_clicked), editor);
    
    // Ajustar parâmetros dos callbacks de formatação
    g_signal_connect_swapped(btn_negrito, "clicked",
                             G_CALLBACK(aplicar_tag), buffer);
    // ... similar para outros botões
    
    gtk_widget_show_all(window);
    gtk_main();
    
    g_free(editor->arquivo_atual);
    g_free(editor);
    
    return 0;
}
```

---

## 8. Desenhando com Cairo {#cairo}

### 8.1 Área de Desenho Básica

```c
#include <gtk/gtk.h>
#include <math.h>

// Estrutura para dados do desenho
typedef struct {
    double x, y;        // Posição do mouse
    gboolean desenhando; // Flag para desenho
    double cor_r, cor_g, cor_b; // Cor atual
} DesenhoDados;

// Callback de desenho (Cairo)
gboolean on_draw(GtkWidget *widget, cairo_t *cr, gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    // Obter dimensões da área de desenho
    GtkAllocation allocation;
    gtk_widget_get_allocation(widget, &allocation);
    int width = allocation.width;
    int height = allocation.height;
    
    // Fundo branco
    cairo_set_source_rgb(cr, 1, 1, 1);
    cairo_paint(cr);
    
    // Desenhar grade
    cairo_set_source_rgb(cr, 0.9, 0.9, 0.9);
    cairo_set_line_width(cr, 0.5);
    
    for (int i = 0; i <= width; i += 20) {
        cairo_move_to(cr, i, 0);
        cairo_line_to(cr, i, height);
    }
    
    for (int i = 0; i <= height; i += 20) {
        cairo_move_to(cr, 0, i);
        cairo_line_to(cr, width, i);
    }
    cairo_stroke(cr);
    
    // Desenhar círculo central
    double center_x = width / 2.0;
    double center_y = height / 2.0;
    double radius = 100.0;
    
    // Gradiente radial
    cairo_pattern_t *grad = cairo_pattern_create_radial(
        center_x - 20, center_y - 20, 20,
        center_x, center_y, radius);
    
    cairo_pattern_add_color_stop_rgb(grad, 0, 1, 0, 0); // Vermelho no centro
    cairo_pattern_add_color_stop_rgb(grad, 0.5, 0, 1, 0); // Verde no meio
    cairo_pattern_add_color_stop_rgb(grad, 1, 0, 0, 1); // Azul na borda
    
    cairo_arc(cr, center_x, center_y, radius, 0, 2 * M_PI);
    cairo_set_source(cr, grad);
    cairo_fill_preserve(cr);
    
    cairo_pattern_destroy(grad);
    
    // Contorno preto
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 2);
    cairo_stroke(cr);
    
    // Desenhar posição do mouse se estiver desenhando
    if (dados->desenhando) {
        cairo_set_source_rgb(cr, dados->cor_r, dados->cor_g, dados->cor_b);
        cairo_arc(cr, dados->x, dados->y, 5, 0, 2 * M_PI);
        cairo_fill(cr);
    }
    
    return FALSE;
}

// Eventos do mouse
gboolean on_motion_notify(GtkWidget *widget, GdkEventMotion *event,
                          gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (dados->desenhando) {
        dados->x = event->x;
        dados->y = event->y;
        gtk_widget_queue_draw(widget); // Solicitar redesenho
    }
    
    return TRUE;
}

gboolean on_button_press(GtkWidget *widget, GdkEventButton *event,
                         gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (event->button == 1) { // Botão esquerdo
        dados->desenhando = TRUE;
        dados->x = event->x;
        dados->y = event->y;
        
        // Mudar cor aleatoriamente
        dados->cor_r = (double)rand() / RAND_MAX;
        dados->cor_g = (double)rand() / RAND_MAX;
        dados->cor_b = (double)rand() / RAND_MAX;
        
        gtk_widget_queue_draw(widget);
    }
    
    return TRUE;
}

gboolean on_button_release(GtkWidget *widget, GdkEventButton *event,
                           gpointer user_data) {
    DesenhoDados *dados = (DesenhoDados *)user_data;
    
    if (event->button == 1) {
        dados->desenhando = FALSE;
        gtk_widget_queue_draw(widget);
    }
    
    return TRUE;
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Inicializar dados de desenho
    DesenhoDados dados;
    dados.x = 0;
    dados.y = 0;
    dados.desenhando = FALSE;
    dados.cor_r = 1;
    dados.cor_g = 0;
    dados.cor_b = 0;
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Desenhando com Cairo");
    gtk_window_set_default_size(GTK_WINDOW(window), 600, 400);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Área de desenho
    GtkWidget *drawing_area = gtk_drawing_area_new();
    gtk_container_add(GTK_CONTAINER(window), drawing_area);
    
    // Habilitar eventos do mouse
    gtk_widget_add_events(drawing_area,
                          GDK_BUTTON_PRESS_MASK |
                          GDK_BUTTON_RELEASE_MASK |
                          GDK_POINTER_MOTION_MASK);
    
    // Conectar sinais
    g_signal_connect(drawing_area, "draw", 
                     G_CALLBACK(on_draw), &dados);
    g_signal_connect(drawing_area, "motion-notify-event",
                     G_CALLBACK(on_motion_notify), &dados);
    g_signal_connect(drawing_area, "button-press-event",
                     G_CALLBACK(on_button_press), &dados);
    g_signal_connect(drawing_area, "button-release-event",
                     G_CALLBACK(on_button_release), &dados);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    return 0;
}
```

### 8.2 Gráficos e Animações

```c
#include <gtk/gtk.h>
#include <math.h>
#include <time.h>

typedef struct {
    double angulo;
    double escala;
    gboolean animando;
    guint timeout_id;
    double pontos[100][2]; // Pontos para o gráfico
    int num_pontos;
} AnimacaoDados;

// Gerar dados do gráfico (seno com ruído)
void gerar_dados(AnimacaoDados *dados) {
    dados->num_pontos = 100;
    for (int i = 0; i < dados->num_pontos; i++) {
        double x = (double)i / (dados->num_pontos - 1) * 2 * M_PI;
        dados->pontos[i][0] = x;
        dados->pontos[i][1] = sin(x) + (rand() % 100) / 500.0;
    }
}

// Callback de animação
gboolean on_timeout(gpointer user_data) {
    AnimacaoDados *dados = (AnimacaoDados *)user_data;
    
    if (dados->animando) {
        dados->angulo += 0.05;
        dados->escala = 0.8 + 0.2 * sin(dados->angulo * 0.5);
        gtk_widget_queue_draw(GTK_WIDGET(dados->drawing_area));
    }
    
    return G_SOURCE_CONTINUE;
}

// Desenhar gráfico
void desenhar_grafico(cairo_t *cr, AnimacaoDados *dados, 
                      int width, int height) {
    // Margens
    double left_margin = 50;
    double right_margin = 50;
    double top_margin = 30;
    double bottom_margin = 40;
    
    double graph_width = width - left_margin - right_margin;
    double graph_height = height - top_margin - bottom_margin;
    
    // Fundo branco para o gráfico
    cairo_set_source_rgb(cr, 1, 1, 1);
    cairo_rectangle(cr, left_margin, top_margin, 
                    graph_width, graph_height);
    cairo_fill(cr);
    
    // Borda do gráfico
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 1);
    cairo_rectangle(cr, left_margin, top_margin, 
                    graph_width, graph_height);
    cairo_stroke(cr);
    
    // Desenhar linhas de grade
    cairo_set_source_rgb(cr, 0.8, 0.8, 0.8);
    cairo_set_line_width(cr, 0.5);
    
    for (int i = 0; i <= 4; i++) {
        double x = left_margin + (i * graph_width / 4);
        cairo_move_to(cr, x, top_margin);
        cairo_line_to(cr, x, top_margin + graph_height);
        
        double y = top_margin + (i * graph_height / 4);
        cairo_move_to(cr, left_margin, y);
        cairo_line_to(cr, left_margin + graph_width, y);
    }
    cairo_stroke(cr);
    
    // Desenhar pontos do gráfico
    if (dados->num_pontos > 0) {
        cairo_set_source_rgb(cr, 0, 0, 1);
        cairo_set_line_width(cr, 2);
        
        for (int i = 0; i < dados->num_pontos - 1; i++) {
            double x1 = left_margin + 
                (dados->pontos[i][0] / (2 * M_PI)) * graph_width;
            double y1 = top_margin + graph_height - 
                ((dados->pontos[i][1] + 1) / 2) * graph_height;
            
            double x2 = left_margin + 
                (dados->pontos[i+1][0] / (2 * M_PI)) * graph_width;
            double y2 = top_margin + graph_height - 
                ((dados->pontos[i+1][1] + 1) / 2) * graph_height;
            
            cairo_move_to(cr, x1, y1);
            cairo_line_to(cr, x2, y2);
        }
        cairo_stroke(cr);
    }
    
    // Rótulos dos eixos
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_select_font_face(cr, "Sans", CAIRO_FONT_SLANT_NORMAL,
                           CAIRO_FONT_WEIGHT_NORMAL);
    cairo_set_font_size(cr, 12);
    
    cairo_move_to(cr, width / 2, height - 10);
    cairo_show_text(cr, "Tempo (s)");
    
    cairo_save(cr);
    cairo_translate(cr, 20, height / 2);
    cairo_rotate(cr, -M_PI / 2);
    cairo_show_text(cr, "Amplitude");
    cairo_restore(cr);
}

gboolean on_draw(GtkWidget *widget, cairo_t *cr, gpointer user_data) {
    AnimacaoDados *dados = (AnimacaoDados *)user_data;
    
    GtkAllocation allocation;
    gtk_widget_get_allocation(widget, &allocation);
    int width = allocation.width;
    int height = allocation.height;
    
    // Fundo
    cairo_set_source_rgb(cr, 0.95, 0.95, 0.95);
    cairo_paint(cr);
    
    // Desenhar gráfico
    desenhar_grafico(cr, dados, width, height);
    
    // Desenhar retângulo animado
    double rect_size = 50 * dados->escala;
    double rect_x = width / 2 - rect_size / 2;
    double rect_y = height / 4 - rect_size / 2;
    
    // Rotação
    cairo_save(cr);
    cairo_translate(cr, rect_x + rect_size / 2, rect_y + rect_size / 2);
    cairo_rotate(cr, dados->angulo);
    cairo_rectangle(cr, -rect_size / 2, -rect_size / 2, 
                    rect_size, rect_size);
    
    // Gradiente para o retângulo
    cairo_pattern_t *grad = cairo_pattern_create_linear(
        -rect_size / 2, -rect_size / 2,
        rect_size / 2, rect_size / 2);
    cairo_pattern_add_color_stop_rgb(grad, 0, 1, 0, 0);
    cairo_pattern_add_color_stop_rgb(grad, 1, 0, 0, 1);
    
    cairo_set_source(cr, grad);
    cairo_fill_preserve(cr);
    cairo_pattern_destroy(grad);
    
    cairo_set_source_rgb(cr, 0, 0, 0);
    cairo_set_line_width(cr, 2);
    cairo_stroke(cr);
    
    cairo_restore(cr);
    
    return FALSE;
}

int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    srand(time(NULL));
    
    AnimacaoDados dados;
    dados.angulo = 0;
    dados.escala = 1;
    dados.animando = TRUE;
    gerar_dados(&dados);
    
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Gráficos e Animação");
    gtk_window_set_default_size(GTK_WINDOW(window), 800, 500);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    
    // Área de desenho
    GtkWidget *drawing_area = gtk_drawing_area_new();
    gtk_box_pack_start(GTK_BOX(vbox), drawing_area, TRUE, TRUE, 0);
    
    dados.drawing_area = drawing_area;
    
    // Barra de controle
    GtkWidget *hbox = gtk_box_new(GTK_ORIENTATION_HORIZONTAL, 5);
    gtk_container_set_border_width(GTK_CONTAINER(hbox), 5);
    gtk_box_pack_start(GTK_BOX(vbox), hbox, FALSE, FALSE, 0);
    
    GtkWidget *btn_iniciar = gtk_button_new_with_label("Iniciar");
    GtkWidget *btn_parar = gtk_button_new_with_label("Parar");
    GtkWidget *btn_novos_dados = gtk_button_new_with_label("Novos Dados");
    
    gtk_box_pack_start(GTK_BOX(hbox), btn_iniciar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox), btn_parar, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(hbox), btn_novos_dados, TRUE, TRUE, 0);
    
    // Conectar sinais
    g_signal_connect(drawing_area, "draw", G_CALLBACK(on_draw), &dados);
    
    g_signal_connect(btn_iniciar, "clicked", 
                     G_CALLBACK(on_iniciar_animacao), &dados);
    g_signal_connect(btn_parar, "clicked", 
                     G_CALLBACK(on_parar_animacao), &dados);
    g_signal_connect(btn_novos_dados, "clicked", 
                     G_CALLBACK(on_novos_dados), &dados);
    
    // Iniciar timer para animação
    dados.timeout_id = g_timeout_add(50, on_timeout, &dados);
    
    gtk_widget_show_all(window);
    gtk_main();
    
    // Limpar timer
    if (dados.timeout_id > 0) {
        g_source_remove(dados.timeout_id);
    }
    
    return 0;
}

void on_iniciar_animacao(GtkButton *button, AnimacaoDados *dados) {
    dados->animando = TRUE;
}

void on_parar_animacao(GtkButton *button, AnimacaoDados *dados) {
    dados->animando = FALSE;
}

void on_novos_dados(GtkButton *button, AnimacaoDados *dados) {
    gerar_dados(dados);
    gtk_widget_queue_draw(GTK_WIDGET(dados->drawing_area));
}
```

---

## 9. Projeto Prático {#projeto}

### Calculadora Completa com GTK

Vamos construir uma calculadora completa que demonstra todos os conceitos aprendidos:

```c
#include <gtk/gtk.h>
#include <math.h>
#include <string.h>
#include <stdlib.h>

// Estrutura da calculadora
typedef struct {
    GtkWidget *entry_display;
    GtkWidget *entry_memoria;
    double valor_atual;
    double memoria;
    char operacao_pendente;
    gboolean novo_numero;
    gboolean ponto_inserido;
    GtkWidget *botoes[20]; // Para referência
} Calculadora;

// Protótipos
void calculadora_adicionar_digito(Calculadora *calc, char digito);
void calculadora_adicionar_ponto(Calculadora *calc);
void calculadora_limpar(Calculadora *calc);
void calculadora_limpar_tudo(Calculadora *calc);
void calculadora_operacao(Calculadora *calc, char op);
void calculadora_igual(Calculadora *calc);
void calculadora_raiz(Calculadora *calc);
void calculadora_porcentagem(Calculadora *calc);
void calculadora_memoria_limpar(Calculadora *calc);
void calculadora_memoria_recall(Calculadora *calc);
void calculadora_memoria_somar(Calculadora *calc);
void calculadora_memoria_subtrair(Calculadora *calc);
void calculadora_atualizar_display(Calculadora *calc);
void calculadora_atualizar_memoria(Calculadora *calc);

// Callbacks
void on_digito_clicked(GtkButton *button, Calculadora *calc) {
    const char *label = gtk_button_get_label(button);
    calculadora_adicionar_digito(calc, label[0]);
}

void on_ponto_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_adicionar_ponto(calc);
}

void on_operacao_clicked(GtkButton *button, Calculadora *calc) {
    const char *label = gtk_button_get_label(button);
    calculadora_operacao(calc, label[0]);
}

void on_igual_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_igual(calc);
}

void on_limpar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_limpar(calc);
}

void on_limpar_tudo_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_limpar_tudo(calc);
}

void on_raiz_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_raiz(calc);
}

void on_porcentagem_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_porcentagem(calc);
}

void on_memoria_limpar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_limpar(calc);
}

void on_memoria_recall_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_recall(calc);
}

void on_memoria_somar_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_somar(calc);
}

void on_memoria_subtrair_clicked(GtkButton *button, Calculadora *calc) {
    calculadora_memoria_subtrair(calc);
}

// Implementação das funções da calculadora
void calculadora_adicionar_digito(Calculadora *calc, char digito) {
    const char *texto_atual = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    
    if (calc->novo_numero) {
        char novo_texto[2] = {digito, '\0'};
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
        calc->novo_numero = FALSE;
        calc->ponto_inserido = FALSE;
    } else {
        char novo_texto[256];
        snprintf(novo_texto, sizeof(novo_texto), "%s%c", texto_atual, digito);
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
    }
}

void calculadora_adicionar_ponto(Calculadora *calc) {
    if (!calc->ponto_inserido) {
        const char *texto_atual = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
        
        if (calc->novo_numero) {
            gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0.");
            calc->novo_numero = FALSE;
        } else {
            char novo_texto[256];
            snprintf(novo_texto, sizeof(novo_texto), "%s.", texto_atual);
            gtk_entry_set_text(GTK_ENTRY(calc->entry_display), novo_texto);
        }
        calc->ponto_inserido = TRUE;
    }
}

void calculadora_limpar(Calculadora *calc) {
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0");
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
}

void calculadora_limpar_tudo(Calculadora *calc) {
    calc->valor_atual = 0;
    calc->operacao_pendente = '\0';
    calculadora_limpar(calc);
}

void calculadora_operacao(Calculadora *calc, char op) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    
    if (calc->operacao_pendente != '\0') {
        calculadora_igual(calc);
        calc->operacao_pendente = op;
    } else {
        calc->valor_atual = valor;
        calc->operacao_pendente = op;
        calc->novo_numero = TRUE;
    }
}

void calculadora_igual(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double segundo_valor = atof(texto);
    double resultado = 0;
    
    switch (calc->operacao_pendente) {
        case '+':
            resultado = calc->valor_atual + segundo_valor;
            break;
        case '-':
            resultado = calc->valor_atual - segundo_valor;
            break;
        case '*':
            resultado = calc->valor_atual * segundo_valor;
            break;
        case '/':
            if (segundo_valor != 0) {
                resultado = calc->valor_atual / segundo_valor;
            } else {
                gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "Erro");
                calc->novo_numero = TRUE;
                calc->operacao_pendente = '\0';
                return;
            }
            break;
        default:
            resultado = segundo_valor;
    }
    
    char resultado_texto[64];
    snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
    
    calc->valor_atual = resultado;
    calc->operacao_pendente = '\0';
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
}

void calculadora_raiz(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    
    if (valor >= 0) {
        double resultado = sqrt(valor);
        char resultado_texto[64];
        snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
        calc->novo_numero = TRUE;
    } else {
        gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "Erro");
        calc->novo_numero = TRUE;
    }
}

void calculadora_porcentagem(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    double resultado = valor / 100.0;
    
    char resultado_texto[64];
    snprintf(resultado_texto, sizeof(resultado_texto), "%g", resultado);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), resultado_texto);
    calc->novo_numero = TRUE;
}

void calculadora_memoria_limpar(Calculadora *calc) {
    calc->memoria = 0;
    calculadora_atualizar_memoria(calc);
}

void calculadora_memoria_recall(Calculadora *calc) {
    char memoria_texto[64];
    snprintf(memoria_texto, sizeof(memoria_texto), "%g", calc->memoria);
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), memoria_texto);
    calc->novo_numero = TRUE;
}

void calculadora_memoria_somar(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    calc->memoria += valor;
    calculadora_atualizar_memoria(calc);
}

void calculadora_memoria_subtrair(Calculadora *calc) {
    const char *texto = gtk_entry_get_text(GTK_ENTRY(calc->entry_display));
    double valor = atof(texto);
    calc->memoria -= valor;
    calculadora_atualizar_memoria(calc);
}

void calculadora_atualizar_memoria(Calculadora *calc) {
    char memoria_texto[64];
    if (calc->memoria != 0) {
        snprintf(memoria_texto, sizeof(memoria_texto), "M: %g", calc->memoria);
    } else {
        snprintf(memoria_texto, sizeof(memoria_texto), "M: 0");
    }
    gtk_entry_set_text(GTK_ENTRY(calc->entry_memoria), memoria_texto);
}

// Função principal
int main(int argc, char *argv[]) {
    gtk_init(&argc, &argv);
    
    // Alocar e inicializar calculadora
    Calculadora *calc = g_new(Calculadora, 1);
    calc->valor_atual = 0;
    calc->memoria = 0;
    calc->operacao_pendente = '\0';
    calc->novo_numero = TRUE;
    calc->ponto_inserido = FALSE;
    
    // Criar janela
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Calculadora GTK");
    gtk_window_set_default_size(GTK_WINDOW(window), 350, 400);
    gtk_window_set_resizable(GTK_WINDOW(window), FALSE);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);
    
    // Container principal
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_set_border_width(GTK_CONTAINER(vbox), 10);
    
    // Display e memória
    calc->entry_display = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(calc->entry_display), "0");
    gtk_entry_set_alignment(GTK_ENTRY(calc->entry_display), 1);
    gtk_entry_set_max_length(GTK_ENTRY(calc->entry_display), 20);
    gtk_widget_set_size_request(calc->entry_display, -1, 50);
    gtk_box_pack_start(GTK_BOX(vbox), calc->entry_display, FALSE, FALSE, 0);
    
    calc->entry_memoria = gtk_entry_new();
    gtk_entry_set_text(GTK_ENTRY(calc->entry_memoria), "M: 0");
    gtk_entry_set_alignment(GTK_ENTRY(calc->entry_memoria), 1);
    gtk_editable_set_editable(GTK_EDITABLE(calc->entry_memoria), FALSE);
    gtk_widget_set_size_request(calc->entry_memoria, -1, 30);
    gtk_box_pack_start(GTK_BOX(vbox), calc->entry_memoria, FALSE, FALSE, 0);
    
    // Grid para botões
    GtkWidget *grid = gtk_grid_new();
    gtk_grid_set_row_spacing(GTK_GRID(grid), 5);
    gtk_grid_set_column_spacing(GTK_GRID(grid), 5);
    gtk_box_pack_start(GTK_BOX(vbox), grid, TRUE, TRUE, 0);
    
    // Definir botões
    const char *botoes[6][5] = {
        {"MC", "MR", "M+", "M-", "C"},
        {"√", "%", "/", "*", "←"},
        {"7", "8", "9", "-", ""},
        {"4", "5", "6", "+", ""},
        {"1", "2", "3", "=", ""},
        {"0", ".", "", "", ""}
    };
    
    // Criar botões
    for (int i = 0; i < 6; i++) {
        for (int j = 0; j < 5; j++) {
            if (strlen(botoes[i][j]) > 0) {
                GtkWidget *button = gtk_button_new_with_label(botoes[i][j]);
                gtk_widget_set_size_request(button, 60, 50);
                gtk_grid_attach(GTK_GRID(grid), button, j, i, 1, 1);
                
                // Conectar sinais baseado no tipo de botão
                const char *label = botoes[i][j];
                
                if (isdigit(label[0])) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_digito_clicked), calc);
                } else if (strcmp(label, ".") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_ponto_clicked), calc);
                } else if (strcmp(label, "=") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_igual_clicked), calc);
                } else if (strcmp(label, "C") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_limpar_clicked), calc);
                } else if (strcmp(label, "←") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_limpar_clicked), calc);
                } else if (strcmp(label, "√") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_raiz_clicked), calc);
                } else if (strcmp(label, "%") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_porcentagem_clicked), calc);
                } else if (strcmp(label, "MC") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_limpar_clicked), calc);
                } else if (strcmp(label, "MR") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_recall_clicked), calc);
                } else if (strcmp(label, "M+") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_somar_clicked), calc);
                } else if (strcmp(label, "M-") == 0) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_memoria_subtrair_clicked), calc);
                } else if (strchr("+-*/", label[0])) {
                    g_signal_connect(button, "clicked", 
                                     G_CALLBACK(on_operacao_clicked), calc);
                }
            }
        }
    }
    
    gtk_widget_show_all(window);
    gtk_main();
    
    g_free(calc);
    return 0;
}
```

---

## 10. Dicas e Boas Práticas {#dicas}

### 10.1 Gerenciamento de Memória

```c
// Sempre liberar memória alocada
gchar *texto = g_strdup_printf("Número: %d", 42);
// ... usar texto
g_free(texto);

// Para objetos GTK, usar g_object_unref quando apropriado
GtkWidget *widget = gtk_button_new();
// ... usar widget
g_object_unref(widget); // Se você tem uma referência extra

// Usar g_clear_pointer para liberar e anular
gchar *dados = g_strdup("importante");
g_clear_pointer(&dados, g_free);
```

### 10.2 Organização de Código

```c
// Separar em arquivos:
// main.c - ponto de entrada
// interface.c - criação da UI
// callbacks.c - funções de callback
// dados.h - estruturas e protótipos

// Exemplo de estrutura de projeto:
/*
projeto/
├── Makefile
├── src/
│   ├── main.c
│   ├── interface.c
│   ├── interface.h
│   ├── callbacks.c
│   ├── callbacks.h
│   └── dados.h
└── resources/
    ├── style.css
    └── icons/
*/
```

### 10.3 CSS para Estilização

```c
// Aplicar CSS personalizado
void aplicar_css(GtkWidget *widget) {
    GtkCssProvider *provider = gtk_css_provider_new();
    gtk_css_provider_load_from_data(provider,
        "button {"
        "   background-color: #3498db;"
        "   color: white;"
        "   border-radius: 5px;"
        "}"
        "button:hover {"
        "   background-color: #2980b9;"
        "}"
        "entry {"
        "   background-color: #ecf0f1;"
        "   border: 1px solid #bdc3c7;"
        "   border-radius: 3px;"
        "}", -1, NULL);
    
    GtkStyleContext *context = gtk_widget_get_style_context(widget);
    gtk_style_context_add_provider(context,
        GTK_STYLE_PROVIDER(provider),
        GTK_STYLE_PROVIDER_PRIORITY_USER);
    
    g_object_unref(provider);
}
```

### 10.4 Internacionalização

```c
#include <libintl.h>
#include <locale.h>

#define _(STRING) gettext(STRING)

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");
    bindtextdomain("meuapp", "/usr/share/locale");
    textdomain("meuapp");
    
    gtk_init(&argc, &argv);
    
    GtkWidget *label = gtk_label_new(_("Hello World"));
    // ...
}
```

### 10.5 Debug e Logging

```c
// Usar g_print para debug
g_print("Valor atual: %f\n", valor);

// Usar g_warning para avisos
g_warning("Operação inválida: %s", operacao);

// Usar g_error para erros fatais
g_error("Falha ao criar widget");

// Usar g_return_if_fail para pré-condições
void minha_funcao(GtkWidget *widget) {
    g_return_if_fail(GTK_IS_WIDGET(widget));
    // ...
}
```

### 10.6 Checklist de Boas Práticas

- [ ] Sempre verificar ponteiros NULL
- [ ] Usar macros de cast (GTK_WIDGET, GTK_WINDOW, etc.)
- [ ] Liberar memória alocada
- [ ] Separar UI da lógica de negócio
- [ ] Usar nomes descritivos para callbacks
- [ ] Documentar funções complexas
- [ ] Tratar erros adequadamente
- [ ] Testar em diferentes resoluções
- [ ] Considerar acessibilidade
- [ ] Seguir as convenções de nomenclatura do GTK

---

## Conclusão

Parabéns! Você agora tem um conhecimento abrangente de GTK em C. Este manual cobriu desde conceitos básicos até tópicos avançados como desenho com Cairo e construção de aplicações completas.

Lembre-se:
- **Pratique** cada exemplo
- **Experimente** modificar os códigos
- **Consulte** a documentação oficial do GTK
- **Participe** da comunidade GTK

Bons códigos e divirta-se criando interfaces incríveis! 🚀
