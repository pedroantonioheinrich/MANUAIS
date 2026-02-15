# 📚 Manual Completo de Programação Assíncrona em Python

## 1. Fundamentos Conceituais

### 1.1 Concorrência vs Paralelismo

**Concorrência:**
- Múltiplas tarefas **compartilhando** recursos em períodos de tempo
- Tarefas **não necessariamente** executam ao mesmo tempo
- Context switching entre tarefas
- Ideal para I/O bound operations

```python
# Exemplo conceitual de concorrência
# Tarefa A: ---[I/O]---[CPU]---[I/O]---
# Tarefa B: ---[CPU]---[I/O]---[CPU]---
# Ambas compartilham a mesma CPU, alternando execução
```

**Paralelismo:**
- Múltiplas tarefas executando **simultaneamente**
- Requer múltiplos processadores/núcleos
- Ideal para CPU bound operations

```python
# Exemplo conceitual de paralelismo
# CPU1: Tarefa A: [CPU][CPU][CPU]
# CPU2: Tarefa B: [CPU][CPU][CPU]
# Execução simultânea em núcleos diferentes
```

### 1.2 Modelos de Programação

**Síncrono (Bloqueante):**
```python
def task_sync():
    result1 = blocking_operation1()  # Espera completa
    result2 = blocking_operation2()  # Espera completa
    return process_results(result1, result2)
```

**Assíncrono (Não-bloqueante):**
```python
async def task_async():
    result1 = await async_operation1()  # Libera controle durante espera
    result2 = await async_operation2()  # Libera controle durante espera
    return process_results(result1, result2)
```

## 2. Async/Await em Python

### 2.1 Fundamentos da Sintaxe

**Palavras-chave:**
- `async`: Declara uma função assíncrona
- `await`: Suspende execução até operação completar
- `async with`: Context manager assíncrono
- `async for`: Loop assíncrono

```python
import asyncio

# Função assíncrona básica
async def minha_funcao():
    print("Iniciando...")
    await asyncio.sleep(1)  # Não bloqueia a thread
    print("Finalizado após 1 segundo")

# Coroutine object (não executa imediatamente)
coro = minha_funcao()

# Execução
asyncio.run(coro)
```

### 2.2 Estados de uma Coroutine

```
┌──────────┐    await    ┌─────────────┐    finish    ┌──────────┐
│ PENDING  │────────────>│ SUSPENDED   │─────────────>│ FINISHED │
└──────────┘             └─────────────┘              └──────────┘
       │                       │
       └───────────────────────┘
           schedule/resume
```

### 2.3 Gerenciando Múltiplas Coroutines

```python
import asyncio

async def tarefa(nome, duracao):
    print(f"{nome}: iniciando")
    await asyncio.sleep(duracao)
    print(f"{nome}: completado em {duracao}s")
    return f"Resultado {nome}"

# Execução sequencial
async def sequencial():
    r1 = await tarefa("A", 2)
    r2 = await tarefa("B", 1)
    print(r1, r2)

# Execução concorrente
async def concorrente():
    # Cria tasks para execução concorrente
    task1 = asyncio.create_task(tarefa("A", 2))
    task2 = asyncio.create_task(tarefa("B", 1))
    
    # Aguarda ambas completarem
    r1 = await task1
    r2 = await task2
    print(r1, r2)

# Usando gather para múltiplas coroutines
async def usando_gather():
    resultados = await asyncio.gather(
        tarefa("A", 2),
        tarefa("B", 1),
        tarefa("C", 3),
        return_exceptions=True  # Não para em caso de exceção
    )
    print(resultados)

# Execução com timeout
async def com_timeout():
    try:
        resultado = await asyncio.wait_for(
            tarefa("Lenta", 5),
            timeout=2.0
        )
    except asyncio.TimeoutError:
        print("Tarefa excedeu o timeout")

asyncio.run(usando_gather())
```

## 3. Asyncio - Framework Assíncrono

### 3.1 Componentes Principais

**Event Loop:**
```python
import asyncio

# Obtendo o event loop
loop = asyncio.get_event_loop()

# Executando coroutines
loop.run_until_complete(minha_funcao())

# Executando forever
async def periodic():
    while True:
        print("Executando periodicamente")
        await asyncio.sleep(1)

# loop.create_task(periodic())
# loop.run_forever()

# Fechando corretamente
try:
    loop.run_forever()
except KeyboardInterrupt:
    pass
finally:
    loop.close()
```

**Tasks:**
```python
async def monitor_tasks():
    # Criando tasks
    task_a = asyncio.create_task(tarefa("A", 2))
    task_b = asyncio.create_task(tarefa("B", 1))
    
    # Verificando estado
    print(f"Task A done: {task_a.done()}")
    print(f"Task B done: {task_b.done()}")
    
    # Aguardando específica
    await task_a
    print(f"Task A done: {task_a.done()}")
    print(f"Task A result: {task_a.result()}")
    
    # Cancelando task
    if not task_b.done():
        task_b.cancel()
        try:
            await task_b
        except asyncio.CancelledError:
            print("Task B foi cancelada")
```

**Futures:**
```python
async def exemplo_future():
    # Criando future
    future = asyncio.Future()
    
    # Simulando resultado assíncrono
    async def setar_resultado():
        await asyncio.sleep(1)
        future.set_result("Resultado do Future")
    
    # Consumindo future
    async def consumir():
        resultado = await future  # Aguarda resultado
        print(f"Recebido: {resultado}")
    
    # Executando
    await asyncio.gather(setar_resultado(), consumir())
```

### 3.2 Synchronization Primitives

```python
import asyncio

async def exemplo_semaforo():
    # Limita concorrência a 2 tasks
    sem = asyncio.Semaphore(2)
    
    async def worker(id):
        async with sem:
            print(f"Worker {id} adquiriu semáforo")
            await asyncio.sleep(1)
            print(f"Worker {id} liberou semáforo")
    
    # Executa 4 workers com limite de 2
    await asyncio.gather(*(worker(i) for i in range(4)))

async def exemplo_lock():
    lock = asyncio.Lock()
    
    async def acessar_recurso(id):
        async with lock:
            print(f"Worker {id} acessando recurso exclusivo")
            await asyncio.sleep(0.5)
            print(f"Worker {id} liberou recurso")
    
    await asyncio.gather(*(acessar_recurso(i) for i in range(3)))

async def exemplo_event():
    event = asyncio.Event()
    
    async def waiter():
        print("Esperando evento...")
        await event.wait()
        print("Evento recebido!")
    
    async def setter():
        await asyncio.sleep(1)
        print("Disparando evento...")
        event.set()
    
    await asyncio.gather(waiter(), waiter(), setter())

async def exemplo_condition():
    cond = asyncio.Condition()
    recurso = None
    
    async def consumidor(id):
        async with cond:
            print(f"Consumidor {id} aguardando...")
            await cond.wait_for(lambda: recurso is not None)
            print(f"Consumidor {id} consumiu: {recurso}")
    
    async def produtor():
        nonlocal recurso
        await asyncio.sleep(1)
        async with cond:
            recurso = "Dados produzidos"
            cond.notify_all()
    
    await asyncio.gather(
        produtor(),
        *(consumidor(i) for i in range(3))
    )
```

### 3.3 Queues Assíncronas

```python
async def exemplo_queue():
    # Queue com limite
    queue = asyncio.Queue(maxsize=2)
    
    async def produtor(id):
        for i in range(3):
            item = f"Item {i} do produtor {id}"
            await queue.put(item)
            print(f"Produzido: {item}")
            await asyncio.sleep(0.1)
        await queue.put(None)  # Sinal de fim
    
    async def consumidor(id):
        while True:
            item = await queue.get()
            if item is None:
                queue.put(None)  # Propaga para outros consumidores
                break
            print(f"Consumidor {id} processou: {item}")
            queue.task_done()
            await asyncio.sleep(0.2)
    
    # Priority Queue
    async def exemplo_priority_queue():
        pq = asyncio.PriorityQueue()
        
        async def adicionar_prioridades():
            await pq.put((2, "Média prioridade"))
            await pq.put((1, "Alta prioridade"))
            await pq.put((3, "Baixa prioridade"))
        
        async def processar():
            while not pq.empty():
                prioridade, item = await pq.get()
                print(f"Processando: {item} (prioridade: {prioridade})")
                pq.task_done()
        
        await asyncio.gather(adicionar_prioridades(), processar())
```

## 4. Threads vs Multiprocessing vs Async

### 4.1 Threads (concorrência com GIL)

```python
import threading
import time
from concurrent.futures import ThreadPoolExecutor

def tarefa_cpu_bound(n):
    # Código CPU intensive - limitado pelo GIL
    resultado = 0
    for i in range(n):
        resultado += i * i
    return resultado

def tarefa_io_bound(n):
    # Código I/O intensive
    time.sleep(n)
    return f"Dormiu por {n} segundos"

# Thread básica
def exemplo_thread_simples():
    thread = threading.Thread(target=tarefa_io_bound, args=(2,))
    thread.start()
    thread.join()

# ThreadPoolExecutor
def exemplo_thread_pool():
    with ThreadPoolExecutor(max_workers=3) as executor:
        # Submetendo tasks
        future1 = executor.submit(tarefa_io_bound, 1)
        future2 = executor.submit(tarefa_io_bound, 2)
        future3 = executor.submit(tarefa_cpu_bound, 1000000)
        
        # Obtendo resultados
        print(future1.result())
        print(future2.result())
        print(future3.result())

# Threads com shared state
class ContadorThreadSafe:
    def __init__(self):
        self.valor = 0
        self._lock = threading.Lock()
    
    def incrementar(self):
        with self._lock:
            self.valor += 1

def worker(contador, n):
    for _ in range(n):
        contador.incrementar()

def exemplo_shared_state():
    contador = ContadorThreadSafe()
    threads = []
    
    for _ in range(10):
        t = threading.Thread(target=worker, args=(contador, 1000))
        threads.append(t)
        t.start()
    
    for t in threads:
        t.join()
    
    print(f"Valor final: {contador.valor}")
```

### 4.2 Multiprocessing (paralelismo real)

```python
import multiprocessing
import time
from concurrent.futures import ProcessPoolExecutor
import os

def tarefa_pesada_cpu(n):
    # Usa CPU intensivamente - beneficia de múltiplos cores
    print(f"Processo {os.getpid()} processando {n}")
    resultado = 0
    for i in range(n * 1000000):
        resultado += i * i
    return resultado

# Processo básico
def exemplo_processo_simples():
    processo = multiprocessing.Process(
        target=tarefa_pesada_cpu,
        args=(100,)
    )
    processo.start()
    processo.join()

# ProcessPoolExecutor
def exemplo_process_pool():
    with ProcessPoolExecutor(max_workers=4) as executor:
        futures = [
            executor.submit(tarefa_pesada_cpu, n)
            for n in range(1, 5)
        ]
        
        for future in futures:
            print(f"Resultado: {future.result()}")

# Compartilhamento de memória entre processos
def exemplo_memoria_compartilhada():
    # Valor compartilhado
    contador = multiprocessing.Value('i', 0)
    
    def worker(val):
        for _ in range(1000):
            with val.get_lock():
                val.value += 1
    
    processos = []
    for _ in range(4):
        p = multiprocessing.Process(target=worker, args=(contador,))
        processos.append(p)
        p.start()
    
    for p in processos:
        p.join()
    
    print(f"Valor final: {contador.value}")

# Queue entre processos
def exemplo_queue_processos():
    def produtor(queue):
        for i in range(5):
            queue.put(f"Item {i}")
            time.sleep(0.1)
        queue.put(None)
    
    def consumidor(queue):
        while True:
            item = queue.get()
            if item is None:
                break
            print(f"Consumido: {item}")
    
    queue = multiprocessing.Queue()
    
    p1 = multiprocessing.Process(target=produtor, args=(queue,))
    p2 = multiprocessing.Process(target=consumidor, args=(queue,))
    
    p1.start()
    p2.start()
    
    p1.join()
    p2.join()
```

### 4.3 Comparativo Detalhado

| Aspecto | Threads | Multiprocessing | Async/Await |
|---------|---------|----------------|-------------|
| **Execução** | Concorrente | Paralela | Concorrente |
| **Memória** | Compartilhada | Isolada | Compartilhada |
| **GIL** | Limitado pelo GIL | Sem GIL | Single-threaded |
| **Ideal para** | I/O operations | CPU operations | I/O operations |
| **Overhead** | Baixo | Alto (fork) | Muito baixo |
| **Comunicação** | Compartilhamento direto | IPC necessário | Direta |
| **Debug** | Difícil (race conditions) | Mais fácil | Moderado |

### 4.4 Combinando Técnicas

```python
import asyncio
import concurrent.futures
import multiprocessing

# Executando código bloqueante em threads a partir de async
async def executar_bloqueante_em_thread():
    loop = asyncio.get_event_loop()
    
    def tarefa_bloqueante():
        # Código bloqueante (ex: requests síncrono, arquivos)
        time.sleep(2)
        return "Resultado da tarefa bloqueante"
    
    # Executa em thread separada
    resultado = await loop.run_in_executor(
        None,  # Usa ThreadPoolExecutor padrão
        tarefa_bloqueante
    )
    print(resultado)

# Usando ProcessPoolExecutor com async
async def executar_cpu_bound_async():
    loop = asyncio.get_event_loop()
    
    def tarefa_cpu_intensiva(n):
        resultado = 0
        for i in range(n * 1000000):
            resultado += i * i
        return resultado
    
    # Cria ProcessPoolExecutor específico
    with concurrent.futures.ProcessPoolExecutor() as pool:
        resultado = await loop.run_in_executor(
            pool,
            tarefa_cpu_intensiva,
            100
        )
        print(f"Resultado CPU intensive: {resultado}")

# Padrão produtor-consumidor híbrido
async def produtor_async(queue_async):
    for i in range(10):
        await queue_async.put(f"Item {i}")
        await asyncio.sleep(0.1)

def consumidor_bloqueante(queue_sync):
    while True:
        item = queue_sync.get()
        if item is None:
            break
        # Processamento CPU intensive
        resultado = sum(i*i for i in range(100000))
        print(f"Processado: {item} -> {resultado}")

async def sistema_hibrido():
    # Queue para comunicação async->sync
    queue_sync = multiprocessing.Queue()
    
    # Inicia consumidores em processos
    processos = []
    for _ in range(2):
        p = multiprocessing.Process(
            target=consumidor_bloqueante,
            args=(queue_sync,)
        )
        p.start()
        processos.append(p)
    
    # Produz dados async
    queue_async = asyncio.Queue()
    prod_task = asyncio.create_task(produtor_async(queue_async))
    
    # Transfere dados async->sync
    while True:
        item = await queue_async.get()
        queue_sync.put(item)
        if item == "Item 9":
            break
    
    # Sinaliza fim
    for _ in range(2):
        queue_sync.put(None)
    
    # Aguarda processos
    for p in processos:
        p.join()
    
    await prod_task
```

## 5. Padrões Avançados e Melhores Práticas

### 5.1 Design Patterns Assíncronos

```python
# Pattern: Connection Pool
class AsyncConnectionPool:
    def __init__(self, max_connections=10):
        self.max_connections = max_connections
        self._connections = asyncio.Queue()
        self._in_use = set()
        self._lock = asyncio.Lock()
    
    async def __aenter__(self):
        self._pool = AsyncConnectionPool(5)
        return self._pool
    
    async def __aexit__(self, *args):
        await self._pool.close()
    
    async def get_connection(self):
        async with self._lock:
            if not self._connections.empty():
                conn = await self._connections.get()
                self._in_use.add(conn)
                return conn
            elif len(self._in_use) < self.max_connections:
                conn = await self._create_connection()
                self._in_use.add(conn)
                return conn
        
        # Aguarda conexão disponível
        conn = await self._connections.get()
        self._in_use.add(conn)
        return conn
    
    async def release_connection(self, conn):
        async with self._lock:
            self._in_use.remove(conn)
            await self._connections.put(conn)
    
    async def _create_connection(self):
        await asyncio.sleep(0.1)  # Simula conexão
        return {"id": id(object()), "connected": True}
    
    async def close(self):
        while not self._connections.empty():
            conn = await self._connections.get()
            await self._close_connection(conn)

# Pattern: Circuit Breaker
class CircuitBreaker:
    def __init__(self, failure_threshold=5, recovery_timeout=30):
        self.failure_threshold = failure_threshold
        self.recovery_timeout = recovery_timeout
        self._failures = 0
        self._state = "CLOSED"  # CLOSED, OPEN, HALF_OPEN
        self._last_failure_time = None
    
    async def execute(self, coro):
        if self._state == "OPEN":
            if self._should_try_recovery():
                self._state = "HALF_OPEN"
            else:
                raise Exception("Circuit breaker is OPEN")
        
        try:
            result = await coro
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise e
    
    def _on_success(self):
        if self._state == "HALF_OPEN":
            self._state = "CLOSED"
        self._failures = 0
    
    def _on_failure(self):
        self._failures += 1
        self._last_failure_time = asyncio.get_event_loop().time()
        
        if self._failures >= self.failure_threshold:
            self._state = "OPEN"
    
    def _should_try_recovery(self):
        if not self._last_failure_time:
            return True
        
        elapsed = asyncio.get_event_loop().time() - self._last_failure_time
        return elapsed > self.recovery_timeout

# Pattern: Rate Limiter
class RateLimiter:
    def __init__(self, calls_per_second=10):
        self.calls_per_second = calls_per_second
        self._queue = asyncio.Queue()
        self._semaphore = asyncio.Semaphore(calls_per_second)
        self._cleanup_task = None
    
    async def __aenter__(self):
        self._cleanup_task = asyncio.create_task(self._cleanup_loop())
        return self
    
    async def __aexit__(self, *args):
        if self._cleanup_task:
            self._cleanup_task.cancel()
    
    async def call(self, coro):
        call_time = asyncio.get_event_loop().time()
        await self._queue.put(call_time)
        
        async with self._semaphore:
            # Remove chamada antiga
            await self._queue.get()
            return await coro
    
    async def _cleanup_loop(self):
        while True:
            await asyncio.sleep(1)
            current_time = asyncio.get_event_loop().time()
            
            # Remove chamadas antigas da queue
            while not self._queue.empty():
                oldest = await self._queue.get()
                if current_time - oldest < 1:
                    await self._queue.put(oldest)
                    break
```

### 5.2 Error Handling em Async

```python
import asyncio
from contextlib import asynccontextmanager

async def tratamento_erros_basico():
    try:
        resultado = await operacao_arriscada()
        return resultado
    except asyncio.TimeoutError:
        print("Timeout ocorreu")
        return None
    except Exception as e:
        print(f"Erro inesperado: {e}")
        raise

# Retry pattern assíncrono
async def retry_async(
    coro,
    max_retries=3,
    delay=1,
    backoff=2,
    exceptions=(Exception,)
):
    last_exception = None
    
    for attempt in range(max_retries + 1):
        try:
            return await coro
        except exceptions as e:
            last_exception = e
            if attempt < max_retries:
                wait_time = delay * (backoff ** attempt)
                print(f"Tentativa {attempt + 1} falhou. "
                      f"Tentando novamente em {wait_time}s")
                await asyncio.sleep(wait_time)
    
    raise last_exception

# Timeout com fallback
async def timeout_with_fallback(coro, timeout, fallback_value=None):
    try:
        return await asyncio.wait_for(coro, timeout)
    except asyncio.TimeoutError:
        print(f"Timeout após {timeout} segundos")
        return fallback_value

# Context manager assíncrono para recursos
@asynccontextmanager
async def managed_resource():
    resource = await acquire_resource()
    try:
        yield resource
    finally:
        await release_resource(resource)

async def usar_recurso():
    async with managed_resource() as resource:
        resultado = await resource.process()
        return resultado
```

### 5.3 Testing Assíncrono

```python
import asyncio
import pytest
from unittest.mock import AsyncMock, patch

# Função assíncrona para testar
async def buscar_dados(url):
    await asyncio.sleep(0.1)
    return {"data": "resultado"}

# Teste com pytest-asyncio
@pytest.mark.asyncio
async def test_buscar_dados():
    resultado = await buscar_dados("http://exemplo.com")
    assert resultado == {"data": "resultado"}

# Mock assíncrono
async def test_com_mock():
    mock_response = AsyncMock()
    mock_response.json = AsyncMock(
        return_value={"data": "mocked"}
    )
    
    with patch('aiohttp.ClientSession.get', 
               return_value=mock_response):
        # Teste sua função
        pass

# Teste com timeout
@pytest.mark.asyncio
async def test_timeout():
    with pytest.raises(asyncio.TimeoutError):
        await asyncio.wait_for(
            buscar_dados("http://lento.com"),
            timeout=0.05
        )

# Teste de concorrência
@pytest.mark.asyncio
async def test_concorrencia():
    tasks = [
        buscar_dados(f"http://exemplo.com/{i}")
        for i in range(5)
    ]
    
    resultados = await asyncio.gather(*tasks)
    assert len(resultados) == 5
```

### 5.4 Performance e Otimização

```python
import asyncio
import time
from functools import wraps

# Decorator para medir performance
def async_timer(func):
    @wraps(func)
    async def wrapper(*args, **kwargs):
        start = time.perf_counter()
        try:
            return await func(*args, **kwargs)
        finally:
            elapsed = time.perf_counter() - start
            print(f"{func.__name__} levou {elapsed:.4f} segundos")
    return wrapper

# Batch processing assíncrono
async def processar_em_batch(items, batch_size=10):
    resultados = []
    
    for i in range(0, len(items), batch_size):
        batch = items[i:i + batch_size]
        
        # Processa batch concorrentemente
        batch_tasks = [
            processar_item(item)
            for item in batch
        ]
        
        batch_resultados = await asyncio.gather(
            *batch_tasks,
            return_exceptions=True
        )
        
        resultados.extend(batch_resultados)
    
    return resultados

# Connection pooling eficiente
async def exemplo_pool_eficiente():
    sem = asyncio.Semaphore(100)  # Limite de concorrência
    
    async def limited_request(url):
        async with sem:
            return await fazer_request(url)
    
    urls = [f"http://exemplo.com/{i}" for i in range(1000)]
    tasks = [limited_request(url) for url in urls]
    
    # Processa em chunks para não sobrecarregar memória
    resultados = []
    for chunk in chunks(tasks, 100):
        chunk_resultados = await asyncio.gather(*chunk)
        resultados.extend(chunk_resultados)
    
    return resultados

def chunks(lst, n):
    """Divide lista em chunks de tamanho n"""
    for i in range(0, len(lst), n):
        yield lst[i:i + n]
```

## 6. Casos de Uso Práticos

### 6.1 Web Scraping Assíncrono

```python
import aiohttp
import asyncio
from bs4 import BeautifulSoup

class AsyncWebScraper:
    def __init__(self, max_concurrent=10):
        self.semaphore = asyncio.Semaphore(max_concurrent)
        self.session = None
    
    async def __aenter__(self):
        self.session = aiohttp.ClientSession()
        return self
    
    async def __aexit__(self, *args):
        if self.session:
            await self.session.close()
    
    async def fetch_url(self, url):
        async with self.semaphore:
            try:
                async with self.session.get(url, timeout=10) as response:
                    if response.status == 200:
                        html = await response.text()
                        return await self.parse_html(html)
                    else:
                        print(f"Erro {response.status} para {url}")
                        return None
            except Exception as e:
                print(f"Erro ao buscar {url}: {e}")
                return None
    
    async def parse_html(self, html):
        soup = BeautifulSoup(html, 'html.parser')
        # Extrai dados...
        return {"title": soup.title.string if soup.title else ""}
    
    async def scrape_multiple(self, urls):
        tasks = [self.fetch_url(url) for url in urls]
        resultados = await asyncio.gather(*tasks, return_exceptions=True)
        return [r for r in resultados if r is not None]

async def exemplo_scraping():
    urls = [
        f"https://exemplo.com/page/{i}"
        for i in range(1, 11)
    ]
    
    async with AsyncWebScraper(max_concurrent=5) as scraper:
        dados = await scraper.scrape_multiple(urls)
        print(f"Coletados {len(dados)} páginas")
```

### 6.2 API Client Assíncrono

```python
import aiohttp
import asyncio
from typing import List, Dict, Any

class AsyncAPIClient:
    def __init__(self, base_url: str, api_key: str = None):
        self.base_url = base_url
        self.api_key = api_key
        self.session = None
        self._rate_limiter = asyncio.Semaphore(10)  # 10 requests/s
    
    async def __aenter__(self):
        headers = {}
        if self.api_key:
            headers["Authorization"] = f"Bearer {self.api_key}"
        
        self.session = aiohttp.ClientSession(
            base_url=self.base_url,
            headers=headers
        )
        return self
    
    async def __aexit__(self, *args):
        if self.session:
            await self.session.close()
    
    async def get(self, endpoint: str, **params):
        async with self._rate_limiter:
            async with self.session.get(endpoint, params=params) as response:
                response.raise_for_status()
                return await response.json()
    
    async def post(self, endpoint: str, data: Dict[str, Any]):
        async with self._rate_limiter:
            async with self.session.post(endpoint, json=data) as response:
                response.raise_for_status()
                return await response.json()
    
    async def batch_get(self, endpoints: List[str]):
        tasks = [self.get(endpoint) for endpoint in endpoints]
        return await asyncio.gather(*tasks, return_exceptions=True)
    
    async def paginated_fetch(self, endpoint: str, page_size: int = 100):
        page = 1
        all_data = []
        
        while True:
            data = await self.get(
                endpoint,
                page=page,
                page_size=page_size
            )
            
            if not data:
                break
            
            all_data.extend(data)
            
            if len(data) < page_size:
                break
            
            page += 1
        
        return all_data
```

### 6.3 WebSocket Assíncrono

```python
import asyncio
import websockets
import json

class WebSocketClient:
    def __init__(self, uri: str):
        self.uri = uri
        self.websocket = None
        self._running = False
        self._message_queue = asyncio.Queue()
    
    async def connect(self):
        self.websocket = await websockets.connect(self.uri)
        self._running = True
        
        # Inicia tasks de recebimento e envio
        self._receive_task = asyncio.create_task(self._receive_messages())
        self._send_task = asyncio.create_task(self._send_messages())
    
    async def disconnect(self):
        self._running = False
        if self.websocket:
            await self.websocket.close()
        
        if hasattr(self, '_receive_task'):
            self._receive_task.cancel()
        if hasattr(self, '_send_task'):
            self._send_task.cancel()
    
    async def send(self, message: dict):
        await self._message_queue.put(message)
    
    async def _receive_messages(self):
        while self._running:
            try:
                message = await self.websocket.recv()
                data = json.loads(message)
                await self.on_message(data)
            except websockets.exceptions.ConnectionClosed:
                print("Conexão WebSocket fechada")
                break
            except Exception as e:
                print(f"Erro ao receber mensagem: {e}")
    
    async def _send_messages(self):
        while self._running:
            try:
                message = await self._message_queue.get()
                await self.websocket.send(json.dumps(message))
            except Exception as e:
                print(f"Erro ao enviar mensagem: {e}")
    
    async def on_message(self, data: dict):
        # Para ser sobrescrito pelo usuário
        print(f"Mensagem recebida: {data}")

async def exemplo_websocket():
    client = WebSocketClient("ws://localhost:8765")
    
    try:
        await client.connect()
        
        # Envia algumas mensagens
        for i in range(10):
            await client.send({"type": "message", "data": f"Hello {i}"})
            await asyncio.sleep(1)
    finally:
        await client.disconnect()
```

## 7. Considerações Finais

### 7.1 Quando Usar Cada Abordagem

**Use Async/Await quando:**
- Aplicações I/O bound (APIs, banco de dados, arquivos)
- Muitas conexões simultâneas (servidores web)
- Operações com alta latência
- Quer economizar recursos de sistema

**Use Threads quando:**
- Precisar integrar com bibliotecas bloqueantes
- Operações I/O com bibliotecas não assíncronas
- Cenários simples de concorrência

**Use Multiprocessing quando:**
- Código CPU intensive
- Processamento paralelo real necessário
- Isolamento completo entre processos necessário

### 7.2 Armadilhas Comuns

1. **Bloquear o Event Loop:**
```python
# ERRADO
async def tarefa():
    time.sleep(5)  # Bloqueia tudo!
    
# CORRETO
async def tarefa():
    await asyncio.sleep(5)  # Não bloqueia
```

2. **Esquecer await:**
```python
# ERRADO
async def processar():
    resultado = funcao_assincrona()  # Esqueceu await!
    
# CORRETO
async def processar():
    resultado = await funcao_assincrona()
```

3. **Misturar padrões incorretamente:**
```python
# Use executors para código bloqueante
resultado = await loop.run_in_executor(None, funcao_bloqueante)
```

### 7.3 Ferramentas Recomendadas

- **asyncio**: Biblioteca padrão para async/await
- **aiohttp**: HTTP client/server assíncrono
- **aiomysql/aiopg**: Drivers assíncronos para MySQL/PostgreSQL
- **websockets**: WebSocket client/server
- **pytest-asyncio**: Testing para código assíncrono
- **uvloop**: Implementação mais rápida do event loop (drop-in replacement)

---

Este manual cobre desde os fundamentos até padrões avançados de programação assíncrona em Python. A escolha entre async/await, threads ou multiprocessing depende do caso de uso específico, e muitas vezes a combinação dessas técnicas fornece a solução mais eficiente.
