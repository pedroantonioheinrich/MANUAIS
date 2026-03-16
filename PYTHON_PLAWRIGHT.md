
## 📘 Manual Completo do Playwright para Python

Este manual foi desenvolvido para fornecer uma compreensão profunda e prática da biblioteca Playwright em Python. Abordaremos desde a instalação até comandos avançados, com explicações detalhadas sobre cada bloco de código.

### **1. Introdução ao Playwright**

O Playwright é uma biblioteca de código aberto desenvolvida pela Microsoft para automação de navegadores. Com uma única API, você pode controlar o **Chromium** (base do Chrome, Edge, Brave), **Firefox** e **WebKit** (base do Safari) . Suas principais vantagens incluem:

-   **Espera Inteligente (Auto-waiting):** Antes de executar uma ação como `click()`, o Playwright espera automaticamente que o elemento esteja visível, habilitado e estável, eliminando a necessidade de `time.sleep()` .
-   **Suporte Multi-linguagem:** APIs consistentes para Python, JavaScript/TypeScript, Java e .NET.
-   **Testes Móveis:** Permite emular dispositivos móveis, geolocalização, idioma e muito mais .
-   **Isolamento:** O conceito de `BrowserContext` permite sessões totalmente isoladas, como navegadores anônimos diferentes.

### **2. Configuração do Ambiente**

Antes de escrever código, é necessário preparar o ambiente.

#### **2.1. Pré-requisitos**
-   **Python 3.8 ou superior** instalado no sistema .
-   **pip** (gerenciador de pacotes do Python).

#### **2.2. Instalação da Biblioteca e dos Navegadores**

Abra seu terminal e execute os seguintes comandos:

```bash
# Instala a biblioteca Playwright
pip install playwright
```

```bash
# Instala os binários dos navegadores (Chromium, Firefox, WebKit)
playwright install
```

Este segundo comando é crucial. Ele baixa versões específicas dos navegadores que o Playwright usará, garantindo consistência .

> **Dica para usuários na China:** Para acelerar o download, configure um mirror antes de executar `playwright install` .
> ```bash
> set PLAYWRIGHT_DOWNLOAD_HOST=https://npmmirror.com/mirrors/playwright
> playwright install
> ```

### **3. Estrutura Básica e Primeiro Script**

O Playwright oferece duas interfaces de programação: **síncrona** e **assíncrona**.

-   **Síncrona (`sync_api`):** Mais fácil para iniciantes e scripts lineares. As operações ocorrem em sequência.
-   **Assíncrona (`async_api`):** Recomendada para projetos que exigem alta performance ou concorrência, como web scrapers que processam múltiplas páginas simultaneamente .

#### **3.1. Exemplo Síncrono (Recomendado para Começar)**

```python
# 1. Importamos o gerenciador síncrono do Playwright
from playwright.sync_api import sync_playwright

# 2. Usamos um context manager (with) para gerenciar o ciclo de vida do Playwright
with sync_playwright() as p:
    # 3. Iniciamos uma instância do navegador (Chromium, firefox ou webkit)
    # O parâmetro 'headless=False' torna o navegador visível. Para rodar em segundo plano, use headless=True.
    browser = p.chromium.launch(headless=False)
    
    # 4. Abrimos uma nova aba/página
    page = browser.new_page()
    
    # 5. Navegamos para uma URL
    page.goto("https://example.com")
    
    # 6. Interagimos com a página (aqui, tiramos um print do título)
    print("Título da página:", page.title())
    
    # 7. Fechamos o navegador para liberar recursos
    browser.close()
```

**Explicação Detalhada:**

-   `from playwright.sync_api import sync_playwright`: Importa a função que inicia o runtime do Playwright .
-   `with sync_playwright() as p`: Cria um contexto. Ao sair deste bloco, o Playwright é encerrado automaticamente.
-   `p.chromium.launch()`: Inicia o processo do navegador. Você pode trocar `chromium` por `firefox` ou `webkit` .
-   `browser.new_page()`: Cria uma nova aba.
-   `page.goto()`: Navega para o endereço especificado.
-   `browser.close()`: Encerra o processo do navegador.

### **4. Comandos Fundamentais da `Page` e `Locator`**

A maior parte da interação com o site acontece através de objetos **`Page`** (a aba) e **`Locator`** (um elemento específico).

#### **4.1. Navegação e Controle da Página**

```python
# Navegar para uma URL
page.goto("https://www.python.org")

# Recarregar a página
page.reload()

# Voltar para a página anterior
page.go_back()

# Avançar para a próxima página
page.go_forward()

# Capturar uma screenshot da página inteira
page.screenshot(path="screenshot.png", full_page=True)
```

#### **4.2. Localizando Elementos com `locator()`**

O método `page.locator()` é a forma principal e mais recomendada de encontrar elementos na página. Ele usa seletores CSS, XPath ou texto . A grande vantagem é que ele retorna um objeto `Locator` que possui **espera automática** embutida.

```python
# Localizar por CSS Selector (recomendado)
campo_busca = page.locator("#id-search-field")  # Por ID
botao = page.locator(".btn-search")            # Por classe

# Localizar por XPath
titulo = page.locator("//h1")

# Localizar por texto exato
link_sobre = page.locator("text=About")

# Localizar por texto parcial (contém)
link_docs = page.locator(":has-text('Doc')")

# Localizar por placeholder
email_input = page.locator("input[placeholder='Digite seu email']")
```

#### **4.3. Ações em Elementos**

```python
# Preencher um campo de texto
page.locator("#nome").fill("João Silva")

# Clicar em um elemento
page.locator("#botao-enviar").click()

# Clicar fora de um elemento ou coordenada específica (para fechar modais, etc.)
page.mouse.click(x=100, y=200)

# Dar um duplo clique
page.locator(".item").dblclick()

# Passar o mouse sobre um elemento (hover)
page.locator("#menu").hover()

# Selecionar uma opção em um dropdown (<select>)
page.locator("#select-pais").select_option("Brasil")  # Por valor visível
page.locator("#select-pais").select_option(value="br") # Por valor do atributo 'value'
page.locator("#select-pais").select_option(index=1)     # Por índice

# Marcar um checkbox ou radio button
page.locator("#aceito-termos").check()
# Desmarcar
page.locator("#aceito-termos").uncheck()

# Simular pressionar uma tecla
page.locator("#campo-busca").press("Enter")
# Ou usar o teclado globalmente
page.keyboard.press("Control+A")
```

#### **4.4. Extraindo Informações**

```python
# Obter o texto de dentro de um elemento
texto_botao = page.locator("button.primary").text_content()
print(texto_botao)

# Obter o valor de um atributo (ex: href de um link)
link = page.locator("a").get_attribute("href")
print(link)

# Obter o valor atual de um campo de input
valor_input = page.locator("#email").input_value()
print(valor_input)

# Contar quantos elementos correspondem a um seletor
quantidade_itens = page.locator(".lista-itens li").count()
print(f"Há {quantidade_itens} itens na lista.")

# Extrair todos os textos de uma lista de elementos
todos_titulos = page.locator("h3.produto-titulo").all_text_contents()
for titulo in todos_titulos:
    print(titulo)

# Verificar se um elemento está visível
if page.locator(".mensagem-sucesso").is_visible():
    print("Ação concluída com sucesso!")
```

### **5. Estratégias de Espera**

Apesar da "espera inteligente" do Playwright, há cenários onde você precisa esperar explicitamente por um estado específico .

```python
# Esperar até que a página atinja um estado de carregamento
# 'load' -> evento load da página
# 'domcontentloaded' -> HTML carregado, mas estilos e imagens ainda podem estar baixando
# 'networkidle' -> nenhuma conexão de rede por pelo menos 500ms (útil para SPAs)
page.wait_for_load_state("networkidle")

# Esperar até que um elemento específico apareça no DOM
page.wait_for_selector("#resultado-da-busca")

# NÃO RECOMENDADO: Esperar um tempo fixo (pode deixar os testes lentos e instáveis)
page.wait_for_timeout(2000)  # 2 segundos
```

### **6. Contextos do Navegador e Múltiplas Abas**

O `BrowserContext` é como uma "sessão anônima" isolada. Cookies, armazenamento local e histórico são compartilhados entre as páginas do mesmo contexto, mas são separados de outros contextos .

```python
with sync_playwright() as p:
    browser = p.chromium.launch()
    
    # Cria um novo contexto de navegador (como uma janela anônima)
    context = browser.new_context()
    
    # Cria páginas dentro deste contexto
    page1 = context.new_page()
    page2 = context.new_page()
    
    page1.goto("https://site1.com")
    page2.goto("https://site2.com")  # Não compartilha cookies com page1? Na verdade, sim, porque estão no mesmo contexto.
    
    # Cria um contexto totalmente novo e isolado
    context2 = browser.new_context()
    page3 = context2.new_page()
    
    # Lidando com abas que abrem sozinhas (ex: link com target="_blank")
    with context.expect_page() as new_page_info:
        page1.locator("a[target='_blank']").click()  # Clique que abre nova aba
    nova_aba = new_page_info.value
    print("Nova aba abriu:", nova_aba.title())
    
    browser.close()
```

### **7. Emular Dispositivos Móveis**

Uma das funcionalidades mais poderosas do Playwright é a capacidade de emular dispositivos móveis com um comando simples .

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    # Acessa o dicionário de dispositivos pré-definidos
    iphone_12 = p.devices["iPhone 12 Pro"]
    
    browser = p.webkit.launch(headless=False)
    
    # Cria um contexto usando as configurações do dispositivo
    # Isso define viewport, user-agent, toque, etc.
    context = browser.new_context(**iphone_12)
    
    page = context.new_page()
    page.goto("https://www.google.com")
    
    # A página agora se comporta como se estivesse em um iPhone
    page.screenshot(path="google-iphone.png")
    
    browser.close()
```

### **8. Assíncrono para Alta Performance**

Para tarefas como web scraping em múltiplas URLs, a versão assíncrona oferece ganhos significativos de velocidade .

```python
import asyncio
from playwright.async_api import async_playwright

async def buscar_titulo(url):
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        await page.goto(url)
        titulo = await page.title()
        await browser.close()
        return titulo

async def main():
    urls = ["https://example.com", "https://python.org", "https://playwright.dev"]
    
    # Cria tarefas para executar as buscas de forma concorrente
    tarefas = [buscar_titulo(url) for url in urls]
    resultados = await asyncio.gather(*tarefas)
    
    for url, titulo in zip(urls, resultados):
        print(f"{url}: {titulo}")

# Executa o loop de eventos
asyncio.run(main())
```

### **9. Depuração e Ferramentas Úteis**

-   **Modo Headless Falso:** Use `launch(headless=False)` para ver o que o navegador está fazendo. Adicione `slow_mo=500` para desacelerar as ações em 500ms e acompanhar visualmente .
    ```python
    browser = p.chromium.launch(headless=False, slow_mo=500)
    ```

-   **Gravador de Código (`codegen`):** Uma ferramenta fantástica para gerar scripts automaticamente. Execute no terminal:
    ```bash
    playwright codegen https://site-alvo.com
    ```
    Uma janela do navegador abrirá. Cada clique ou ação que você fizer será convertido em código Python na janela do Playwright Inspector .

-   **Playwright Inspector:** Ferramenta gráfica para depuração. Você pode pausar a execução, percorrer cada passo e ver os seletores.
    ```python
    # Para abrir o inspector, configure a variável de ambiente antes de executar
    # No terminal: set PWDEBUG=1 (Windows) ou export PWDEBUG=1 (Mac/Linux)
    # Depois execute seu script Python
    page.locator("button").click()  # A execução pausará aqui.
    ```

-   **Rastreamento (Tracing):** Para falhas intermitentes, grave um trace e analise-o em um visualizador web .
    ```python
    context = browser.new_context()
    # Inicia a gravação do trace
    context.tracing.start(screenshots=True, snapshots=True)
    
    page = context.new_page()
    page.goto("https://example.com")
    # ... suas ações ...
    
    # Para e salva o trace
    context.tracing.stop(path="trace.zip")
    ```
    Para visualizar: `npx playwright show-trace trace.zip`

