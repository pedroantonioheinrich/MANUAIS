PYTHON# Manual Completo sobre Métodos de Espera (Wait) no Playwright para Python

## 1. Introdução

O Playwright é uma biblioteca de automação de navegadores que se destaca por sua abordagem **inteligente e assíncrona**. Ao contrário de ferramentas tradicionais que exigem esperas arbitrárias com `time.sleep()`, o Playwright incorpora **auto-waiting** – uma série de verificações automáticas antes de executar ações, garantindo que os elementos estejam prontos para interação.

Este manual explora todos os métodos de espera disponíveis no Playwright para Python, desde as esperas automáticas integradas até as esperas explícitas mais refinadas. Compreender esses mecanismos é fundamental para criar testes e robôs confiáveis e eficientes.

---

## 2. Auto-Waiting (Espera Automática)

O Playwright aplica automaticamente um conjunto de verificações antes de realizar uma ação em um elemento (como clicar, digitar, etc.). Essas verificações garantem que o elemento esteja:

- **Anexado ao DOM**.
- **Visível** (não oculto por CSS ou posicionamento).
- **Estável** (não está animado ou em movimento).
- **Habilitado** (não desabilitado, se a ação exigir).
- **Recebendo eventos** (para ações como clique).

Se alguma condição não for satisfeita dentro do tempo limite (timeout), o Playwright lançará uma exceção.

**Exemplo:**  
```python
# O Playwright aguarda automaticamente que o botão esteja visível e habilitado
page.click("button#submit")
```

Esse comportamento elimina a necessidade de esperas explícitas na maioria dos casos, mas ainda existem situações onde precisamos controlar o fluxo manualmente.

---

## 3. Esperas Explícitas

As esperas explícitas permitem pausar a execução até que uma condição específica seja satisfeita. O Playwright oferece diversos métodos para isso.

### 3.1. `page.wait_for_load_state(state, timeout)`

Aguarda até que a página atinja um determinado estado de carregamento. Útil após navegações ou ações que disparam recarregamentos parciais.

**Parâmetros:**
- `state` (opcional): pode ser `"load"`, `"domcontentloaded"` ou `"networkidle"`. Padrão: `"load"`.
  - `"load"`: evento `load` disparado.
  - `"domcontentloaded"`: evento `DOMContentLoaded` disparado.
  - `"networkidle"`: nenhuma requisição de rede ativa por pelo menos 500 ms (útil para SPAs).
- `timeout` (opcional): tempo máximo em milissegundos. Padrão: 30000 (30s).

```python
# Aguarda o carregamento completo (load)
page.wait_for_load_state()

# Aguarda até que a rede esteja ociosa
page.wait_for_load_state("networkidle", timeout=10000)
```

### 3.2. `page.wait_for_url(url, options)`

Aguarda até que a URL da página corresponda ao padrão esperado.

**Parâmetros:**
- `url` (obrigatório): string ou regex (como `re.compile(r".*/success")`).
- `options`:
  - `timeout`: tempo limite.
  - `wait_until`: pode ser `"load"`, `"domcontentloaded"`, `"networkidle"`.

```python
import re
page.wait_for_url("https://exemplo.com/perfil")
page.wait_for_url(re.compile(r".*dashboard"), timeout=5000)
```

### 3.3. `page.wait_for_function(expression, options)`

Executa uma função JavaScript no contexto da página e aguarda até que ela retorne um valor truthy.

**Parâmetros:**
- `expression` (obrigatório): string com código JavaScript.
- `options`:
  - `timeout`
  - `polling`: intervalo entre tentativas (em ms) ou `"raf"` (requestAnimationFrame). Padrão: `100`.
  - `arg`: argumento a ser passado para a função.

```python
# Aguarda até que um elemento com a classe "loaded" apareça
page.wait_for_function('document.querySelector(".loaded") !== null')

# Com polling mais frequente
page.wait_for_function('window.meuFlag === true', polling=50)

# Passando argumento
page.wait_for_function('(valor) => document.getElementById("contador").innerText === valor', arg="100")
```

### 3.4. `page.wait_for_timeout(milliseconds)`

Aguarda um número fixo de milissegundos. **Evite usar** a menos que seja estritamente necessário. Prefira esperas baseadas em condições.

```python
page.wait_for_timeout(2000)  # aguarda 2 segundos
```

### 3.5. `page.wait_for_selector(selector, options)`

Aguarda até que um elemento correspondente ao seletor apareça no DOM. Retorna o elemento (ou `None` se não encontrado).  
**Nota:** esse método está obsoleto em favor de `locator.wait_for()`.

**Parâmetros:**
- `selector` (obrigatório): seletor CSS, texto ou XPath.
- `options`:
  - `state`: pode ser `"attached"`, `"detached"`, `"visible"` ou `"hidden"`. Padrão: `"visible"`.
  - `timeout`: tempo limite.

```python
# Aguarda até que um botão com texto "Salvar" esteja visível
page.wait_for_selector("button:has-text('Salvar')", state="visible")
```

### 3.6. `page.wait_for_event(event, options)`

Aguarda até que um evento específico seja disparado na página. Retorna o objeto do evento.

```python
# Aguarda um diálogo (alert, confirm, prompt)
with page.expect_event("dialog") as dialog_info:
    page.click("button#mostrar-dialogo")
dialog = dialog_info.value
print(dialog.message)

# Aguarda um popup (nova janela)
with page.expect_event("popup") as popup_info:
    page.click("a[target='_blank']")
popup = popup_info.value
```

### 3.7. `page.wait_for_request(url_or_predicate, options)`

Aguarda o envio de uma requisição HTTP que corresponda ao padrão.

```python
# Aguarda requisição para /api/salvar
with page.expect_request("**/api/salvar") as request_info:
    page.click("button#salvar")
request = request_info.value
```

### 3.8. `page.wait_for_response(url_or_predicate, options)`

Aguarda uma resposta HTTP que corresponda ao padrão.

```python
with page.expect_response("**/api/salvar") as response_info:
    page.click("button#salvar")
response = response_info.value
assert response.status == 200
```

---

## 4. Esperas em Localizadores (Locators)

Os localizadores são a forma moderna e recomendada de interagir com elementos. Eles também possuem métodos de espera.

### 4.1. `locator.wait_for(options)`

Aguarda até que o elemento localizado atinja um estado específico.

**Parâmetros:**
- `state`: `"attached"`, `"detached"`, `"visible"`, `"hidden"`. Padrão: `"visible"`.
- `timeout`: tempo limite.

```python
locator = page.locator("#resultado")
locator.wait_for(state="visible")
```

### 4.2. `locator.wait_for_element_state(state, timeout)`

Sinônimo de `wait_for()` com parâmetro `state`. Mantido por compatibilidade.

```python
locator.wait_for_element_state("visible")
```

### 4.3. `locator.wait_for_selector(selector, options)`

Não existe diretamente; use `page.wait_for_selector()` ou combine localizadores.

---

## 5. Navegação e Esperas

O método `page.goto(url)` também possui o parâmetro `wait_until`, que define quando a navegação é considerada concluída.

**Exemplo:**
```python
page.goto("https://exemplo.com", wait_until="networkidle")
```

Valores possíveis para `wait_until`:
- `"load"`: evento `load` (padrão).
- `"domcontentloaded"`: evento `DOMContentLoaded`.
- `"networkidle"`: nenhuma requisição de rede ativa por 500 ms.
- `"commit"`: quando a navegação foi confirmada (o documento começa a ser recebido).

---

## 6. Configuração de Timeout Global

Você pode definir um timeout global para todas as operações de espera e ações.

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    context = browser.new_context()
    page = context.new_page()
    page.set_default_timeout(10000)  # 10 segundos para todas as operações

    # Agora todas as esperas terão timeout de 10s, a menos que sobrescritas
    page.goto("https://exemplo.com")
    page.click("button#submit")  # aguardará até 10s
```

Também é possível definir timeout no próprio método:

```python
page.click("button#submit", timeout=5000)
```

---

## 7. Exemplos Práticos

### 7.1. Aguardar um Elemento Aparecer e Clicar

```python
# Usando auto-waiting (recomendado)
page.click("button#finalizar")  # aguarda automaticamente

# Com espera explícita
botao = page.locator("button#finalizar")
botao.wait_for()
botao.click()
```

### 7.2. Aguardar até que uma API Responda

```python
with page.expect_response("**/api/usuarios") as response_info:
    page.click("button#carregar")
response = response_info.value
assert response.status == 200
dados = response.json()
```

### 7.3. Aguardar uma Condição Customizada com JavaScript

```python
# Aguarda até que o contador tenha valor >= 100
page.wait_for_function('document.getElementById("contador").innerText >= 100')
```

### 7.4. Aguardar o Fim de uma Animação

```python
# Aguarda que a classe "animando" seja removida do elemento
page.wait_for_function('document.querySelector(".meu-elemento").classList.contains("animando") === false')
```

### 7.5. Aguardar Múltiplas Requisições com `expect_request` e `wait_for`

```python
with page.expect_request("**/api/login") as req_login:
    with page.expect_response("**/api/usuarios") as resp_usuarios:
        page.click("button#entrar")
```

---

## 8. Boas Práticas

1. **Prefira esperas automáticas:** deixe o Playwright gerenciar a espera na maioria das ações.
2. **Use localizadores (locators)** em vez de seletores diretos para maior robustez.
3. **Evite `wait_for_timeout`:** em vez disso, aguarde condições reais (elemento visível, requisição, etc.).
4. **Defina timeouts adequados:** nem muito baixo (causa falsos negativos) nem muito alto (torna os testes lentos).
5. **Trate exceções de timeout:** utilize blocos `try/except` para lidar com erros de espera.
6. **Use `networkidle` com cautela:** em SPAs com requisições constantes, pode nunca atingir o estado ocioso; prefira aguardar elementos específicos.
7. **Aproveite `expect_*` para ações que disparam eventos:** isso garante que o evento tenha sido capturado antes de continuar.

---

## 9. Conclusão

Os métodos de espera no Playwright oferecem um controle refinado sobre a sincronização entre o script e a página, ao mesmo tempo que eliminam a fragilidade de esperas arbitrárias. Combinando o auto-waiting com esperas explícitas, você pode construir automações confiáveis e eficientes.

Este manual cobriu os principais métodos de espera, suas aplicações e boas práticas. Para mais detalhes, consulte a [documentação oficial do Playwright](https://playwright.dev/python/docs/api/class-page).

---

**Versão:** 1.0  
**Data:** Março de 2026
