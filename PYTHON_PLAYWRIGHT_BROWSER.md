# Manual Completo de Browser no Playwright com Python

## Índice
1. [Introdução](#introdução)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Tipos de Browser](#tipos-de-browser)
4. [Gerenciamento de Browser](#gerenciamento-de-browser)
5. [Contextos de Navegação](#contextos-de-navegação)
6. [Páginas e Abas](#páginas-e-abas)
7. [Configurações Avançadas](#configurações-avançadas)
8. [Melhores Práticas](#melhores-práticas)

## Introdução

O Playwright é uma ferramenta de automação de browsers que suporta Chromium, Firefox e WebKit. Ele permite controlar browsers programaticamente com uma API unificada e recursos avançados.

### Principais Características
- Suporte multi-browser (Chromium, Firefox, WebKit)
- API assíncrona e síncrona
- Auto-waiting para elementos
- Isolamento de contexto
- Suporte a mobile e geolocalização

## Instalação e Configuração

### Instalação Básica
```python
# Instalar o Playwright
pip install playwright

# Instalar os browsers
playwright install
```

### Estrutura Básica
```python
from playwright.sync_api import sync_playwright

def main():
    with sync_playwright() as p:
        # Código de automação aqui
        browser = p.chromium.launch(headless=False)
        page = browser.new_page()
        page.goto("https://example.com")
        browser.close()

if __name__ == "__main__":
    main()
```

## Tipos de Browser

### Chromium
```python
# Chromium (padrão)
browser = playwright.chromium.launch()

# Chrome (canal específico)
browser = playwright.chromium.launch(
    channel="chrome",  # ou "msedge" para Edge
    headless=False
)
```

### Firefox
```python
# Firefox
browser = playwright.firefox.launch(
    headless=False,
    firefox_user_prefs={
        "dom.push.enabled": False
    }
)
```

### WebKit
```python
# WebKit (Safari)
browser = playwright.webkit.launch(
    headless=False,
    device_scale_factor=2  # Para retina displays
)
```

## Gerenciamento de Browser

### Launch Options
```python
browser = playwright.chromium.launch(
    headless=False,  # Modo headless
    slow_mo=100,     # Delay entre operações (ms)
    timeout=30000,   # Timeout de inicialização
    args=[           # Argumentos adicionais
        '--disable-blink-features=AutomationControlled',
        '--disable-dev-shm-usage',
        '--no-sandbox',
        '--disable-setuid-sandbox',
        '--disable-web-security',
        '--disable-features=IsolateOrigins,site-per-process'
    ],
    proxy={
        "server": "http://myproxy.com:3128",
        "username": "user",
        "password": "pass"
    },
    downloads_path="./downloads",
    env={
        "CUSTOM_VAR": "value"
    }
)
```

### Browser Contextos
```python
# Contexto padrão
context = browser.new_context()

# Contexto com configurações específicas
context = browser.new_context(
    viewport={'width': 1920, 'height': 1080},
    user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    locale='pt-BR',
    timezone_id='America/Sao_Paulo',
    permissions=['geolocation'],
    geolocation={'latitude': -23.5505, 'longitude': -46.6333},
    device_scale_factor=1,
    color_scheme='dark',  # 'light' ou 'dark'
    extra_http_headers={
        'X-Custom-Header': 'Value'
    }
)
```

## Contextos de Navegação

### Criando e Gerenciando Contextos
```python
# Múltiplos contextos
context1 = browser.new_context()
context2 = browser.new_context()

# Persistência de estado
storage_state = context1.storage_state(path="state.json")
context2 = browser.new_context(storage_state="state.json")

# Limpar contexto
context.clear_cookies()
context.clear_permissions()
```

### Autenticação e Sessões
```python
# Autenticação básica
context = browser.new_context(
    http_credentials={
        "username": "user",
        "password": "pass"
    }
)

# Salvar estado de autenticação
def save_auth_state():
    with sync_playwright() as p:
        browser = p.chromium.launch()
        context = browser.new_context()
        page = context.new_page()
        page.goto("https://example.com/login")
        # Realizar login
        page.fill("#username", "user")
        page.fill("#password", "pass")
        page.click("#login")
        page.wait_for_load_state("networkidle")
        
        # Salvar estado
        context.storage_state(path="auth.json")
        browser.close()
```

## Páginas e Abas

### Gerenciamento de Páginas
```python
# Criar nova página
page = context.new_page()

# Configurar página
page.set_viewport_size({"width": 1280, "height": 720})
page.set_extra_http_headers({
    "Authorization": "Bearer token"
})

# Navegação
page.goto("https://example.com", wait_until="networkidle")

# Recarregar
page.reload()

# Voltar/avançar
page.go_back()
page.go_forward()
```

### Trabalhando com Múltiplas Abas
```python
# Aguardar nova aba
with context.expect_page() as new_page_info:
    page.click("a[target='_blank']")
new_page = new_page_info.value

# Listar todas as páginas
pages = context.pages
for i, p in enumerate(pages):
    print(f"Página {i}: {p.url}")

# Fechar página específica
pages[1].close()

# Trabalhar com popups
def handle_dialog(dialog):
    print(f"Dialog message: {dialog.message}")
    dialog.accept()

page.on("dialog", handle_dialog)
```

## Configurações Avançadas

### Dispositivos Móveis
```python
# Emular dispositivos
iphone_13 = playwright.devices['iPhone 13']
context = browser.new_context(**iphone_13)

# Configuração personalizada para mobile
context = browser.new_context(
    viewport={'width': 390, 'height': 844},
    user_agent='Mozilla/5.0 (iPhone; CPU iPhone OS 15_0 like Mac OS X) AppleWebKit/605.1.15',
    device_scale_factor=3,
    is_mobile=True,
    has_touch=True
)
```

### Geolocalização e Permissões
```python
# Configurar geolocalização
context = browser.new_context(
    geolocation={'latitude': -23.5505, 'longitude': -46.6333},
    permissions=['geolocation']
)

page = context.new_page()
page.goto("https://maps.google.com")

# Modificar geolocalização dinamicamente
page.context.set_geolocation({
    'latitude': -22.9068,
    'longitude': -43.1729
})
```

### Redes e Interceptação
```python
# Interceptar requisições
def handle_route(route):
    if route.request.resource_type == "image":
        route.abort()
    else:
        route.continue_()

page.route("**/*", handle_route)

# Monitorar requisições
page.on("request", lambda request: print(f"Request: {request.url}"))
page.on("response", lambda response: print(f"Response: {response.status}"))

# Simular offline
page.context.set_offline(True)
```

### Vídeos e Traces
```python
# Gravar vídeo
context = browser.new_context(
    record_video_dir="videos/",
    record_video_size={"width": 1280, "height": 720}
)

# Gravar trace
context.tracing.start(screenshots=True, snapshots=True)
# ... execução ...
context.tracing.stop(path="trace.zip")

# Capturar screenshot
page.screenshot(path="screenshot.png", full_page=True)
```

## Melhores Práticas

### Gerenciamento de Recursos
```python
# Usar context manager
with sync_playwright() as p:
    with p.chromium.launch() as browser:
        with browser.new_context() as context:
            with context.new_page() as page:
                page.goto("https://example.com")
                # Automação aqui
```

### Tratamento de Erros
```python
from playwright.sync_api import TimeoutError, Error

try:
    page.goto("https://example.com", timeout=5000)
    page.click("#button", timeout=3000)
except TimeoutError:
    print("Timeout ao carregar página ou clicar no botão")
except Error as e:
    print(f"Erro do Playwright: {e}")
finally:
    if browser:
        browser.close()
```

### Estratégias de Espera
```python
# Auto-waiting (recomendado)
page.click("#dynamic-button")  # Aguarda automaticamente

# Esperas explícitas
page.wait_for_selector(".loading-complete", timeout=5000)
page.wait_for_load_state("networkidle")
page.wait_for_function("() => document.readyState === 'complete'")

# Espera condicional
def check_condition():
    return page.locator(".result").count() > 0

page.wait_for_function(check_condition)
```

### Performance e Escalabilidade
```python
# Reutilizar contextos quando possível
class BrowserManager:
    def __init__(self):
        self.playwright = None
        self.browser = None
        self.context = None
    
    def start(self):
        self.playwright = sync_playwright().start()
        self.browser = self.playwright.chromium.launch()
        self.context = self.browser.new_context()
        return self.context
    
    def stop(self):
        if self.context:
            self.context.close()
        if self.browser:
            self.browser.close()
        if self.playwright:
            self.playwright.stop()

# Pool de contexts para paralelismo
from concurrent.futures import ThreadPoolExecutor

def process_page(context, url):
    page = context.new_page()
    page.goto(url)
    # Processar página
    page.close()

contexts = [browser.new_context() for _ in range(5)]
with ThreadPoolExecutor(max_workers=5) as executor:
    futures = [executor.submit(process_page, ctx, url) 
               for ctx, url in zip(contexts, urls)]
```

### Segurança e Anti-Detecção
```python
# Evitar detecção de automação
browser = playwright.chromium.launch(
    args=[
        '--disable-blink-features=AutomationControlled',
        '--disable-features=IsolateOrigins,site-per-process',
        '--disable-web-security'
    ]
)

context = browser.new_context(
    user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
    viewport={'width': 1920, 'height': 1080},
    locale='pt-BR',
    timezone_id='America/Sao_Paulo'
)

# Remover webdriver property
page.add_init_script("""
    Object.defineProperty(navigator, 'webdriver', {
        get: () => undefined
    });
""")
```

### Debugging
```python
# Modo headless false com slow_mo
browser = playwright.chromium.launch(
    headless=False,
    slow_mo=500
)

# Console logging
page.on("console", lambda msg: print(f"Console: {msg.text}"))

# Capturar HTML para debug
def debug_state(page, name):
    page.screenshot(path=f"{name}.png")
    with open(f"{name}.html", "w") as f:
        f.write(page.content())

# Utilizar o Playwright Inspector
browser = playwright.chromium.launch(
    headless=False,
    env={"PWDEBUG": "1"}
)
```

Este manual cobre os principais aspectos do gerenciamento de browsers no Playwright com Python. A ferramenta oferece muito mais recursos, e recomenda-se consultar a documentação oficial para casos específicos e atualizações mais recentes.
