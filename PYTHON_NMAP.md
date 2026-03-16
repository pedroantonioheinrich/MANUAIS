# Manual Completo do Módulo `python-nmap`

## Sumário
1. [Introdução](#introdução)
2. [Instalação](#instalação)
3. [Primeiros Passos](#primeiros-passos)
4. [O Método `scan()`](#o-método-scan)
5. [Interpretando os Resultados](#interpretando-os-resultados)
6. [Varreduras Assíncronas com `PortScannerAsync`](#varreduras-assíncronas-com-portscannerasync)
7. [Argumentos Avançados do Nmap](#argumentos-avançados-do-nmap)
8. [Tratamento de Erros e Exceções](#tratamento-de-erros-e-exceções)
9. [Dicas de Fluxo e Boas Práticas](#dicas-de-fluxo-e-boas-práticas)
10. [Exemplos Práticos](#exemplos-práticos)
11. [Considerações Finais](#considerações-finais)

---

## Introdução

O **python-nmap** é um módulo Python que atua como um wrapper para a ferramenta de linha de comando **Nmap** (Network Mapper). Ele permite que você execute varreduras de rede diretamente de scripts Python e manipule os resultados de forma programática, utilizando estruturas de dados nativas como dicionários e listas.

Com esse módulo, você pode automatizar tarefas de descoberta de hosts, mapeamento de portas, detecção de serviços e sistema operacional, além de integrar essas funcionalidades em aplicações maiores, como ferramentas de monitoramento ou scripts de segurança.

**Pré‑requisitos:**  
- Ter o **Nmap** instalado no sistema (a partir da versão 7.0).  
- Ter o Python 2.7 ou 3.x.  
- Conhecimentos básicos de Python e redes.

---

## Instalação

A maneira mais simples é usar o `pip`:

```bash
pip install python-nmap
```

Caso prefira, também é possível baixar o código fonte do [repositório oficial](https://github.com/bitbucket/bitbucket) e instalá‑lo manualmente.

Após a instalação, verifique se o módulo pode ser importado:

```python
import nmap
print(nmap.__version__)
```

---

## Primeiros Passos

Comece importando o módulo e criando um objeto da classe principal `PortScanner`:

```python
import nmap

nm = nmap.PortScanner()
```

Esse objeto representa uma varredura síncrona (bloqueante). Para varreduras assíncronas (não bloqueantes), usaremos `PortScannerAsync` mais adiante.

---

## O Método `scan()`

O método mais importante é `scan()`, que dispara uma varredura com os parâmetros fornecidos.

### Sintaxe básica

```python
nm.scan(hosts='127.0.0.1', ports='22-443', arguments='-sV')
```

- **hosts**: alvo(s) da varredura. Pode ser um endereço IP, um range (ex.: `'192.168.1.1-20'`), um CIDR (`'192.168.1.0/24'`) ou um nome de domínio.
- **ports**: portas a serem escaneadas. Ex.: `'22'`, `'80,443,8080'`, `'1-1024'`.
- **arguments**: argumentos extras do Nmap (ex.: `'-sS'` para SYN stealth, `'-O'` para detecção de SO, `'-sV'` para detecção de versão de serviços). Se não for especificado, usa um padrão leve `'-sS'` (depende de privilégios).

> **Atenção:** Se você usar varreduras que exigem privilégios de root (como `-sS` ou `-O`), execute o script Python com `sudo`.

### Exemplo mínimo

```python
import nmap

nm = nmap.PortScanner()
resultado = nm.scan('scanme.nmap.org', '22-80')
print(resultado)
```

O retorno de `scan()` é um dicionário com todos os detalhes da varredura. No entanto, a maneira mais comum de acessar os dados é através dos métodos e atributos do objeto `PortScanner`.

---

## Interpretando os Resultados

Após executar `scan()`, o objeto `PortScanner` populado permite acessar as informações de diversas formas.

### Estrutura dos dados

Internamente, os resultados são armazenados em um dicionário (`nm.scaninfo()` e `nm.all_hosts()`). Mas a forma mais prática é usar os métodos da classe.

#### `nm.all_hosts()`
Retorna uma lista com todos os endereços IP (ou nomes) dos hosts encontrados na varredura.

#### `nm[host]`
Acessa as informações de um host específico, como se fosse um dicionário. Esse objeto possui sub‑níveis para protocolos e portas.

#### `nm[host].hostname()`
Retorna o nome DNS do host, se disponível.

#### `nm[host].state()`
Estado do host: `'up'` ou `'down'`.

#### `nm[host].all_protocols()`
Lista os protocolos detectados no host (ex.: `['tcp', 'udp']`).

#### `nm[host][proto]`
Acessa as portas de um protocolo. Por exemplo, `nm['192.168.1.1']['tcp']` é um dicionário cujas chaves são os números das portas e os valores são outros dicionários com informações da porta.

Para cada porta, temos:
- `state`: estado da porta (`'open'`, `'closed'`, `'filtered'`).
- `name`: nome do serviço associado (ex.: `'ssh'`, `'http'`).
- `product`: produto/software que roda na porta (quando disponível).
- `version`: versão do produto.
- `extrainfo`: informações adicionais.
- `conf`: nível de confiança da detecção.
- `cpe`: identificador CPE (Common Platform Enumeration).

#### Exemplo de acesso

```python
import nmap

nm = nmap.PortScanner()
nm.scan('scanme.nmap.org', '22-80')

for host in nm.all_hosts():
    print('Host: %s (%s)' % (host, nm[host].hostname()))
    print('Estado: %s' % nm[host].state())
    for proto in nm[host].all_protocols():
        print('Protocolo: %s' % proto)
        portas = nm[host][proto].keys()
        for porta in sorted(portas):
            info = nm[host][proto][porta]
            print('  Porta: %s\tEstado: %s\tServiço: %s' % (porta, info['state'], info['name']))
```

### Método `scaninfo()`

Retorna informações sobre a varredura em si (argumentos utilizados, tempo, etc.):

```python
print(nm.scaninfo())
```

Exemplo de saída:
```
{'tcp': {'services': '22-80', 'method': 'connect'}}
```

### Atributo `command_line`

Mostra o comando Nmap que foi executado:

```python
print(nm.command_line())
```

---

## Varreduras Assíncronas com `PortScannerAsync`

Quando você precisa escanear muitos hosts ou portas, uma varredura síncrona pode travar seu programa até que termine. O módulo oferece a classe `PortScannerAsync` para executar a varredura em segundo plano, chamando uma função de callback quando cada host é concluído.

### Exemplo básico

```python
import nmap

def callback_result(host, scan_result):
    print('Host: %s retornou: %s' % (host, scan_result))

nm = nmap.PortScannerAsync()
nm.scan(hosts='192.168.1.0/30', arguments='-sV', callback=callback_result)

# Aguarda a conclusão (opcional, para não sair do programa)
while nm.still_scanning():
    print("Escaneando...")
    nm.wait(2)   # espera 2 segundos e continua
```

- `scan()` do `PortScannerAsync` recebe os mesmos parâmetros `hosts`, `ports`, `arguments`, e mais um parâmetro obrigatório `callback`, que deve ser uma função.
- A função de callback será chamada para cada host individualmente assim que seus resultados estiverem disponíveis. Ela recebe dois argumentos: o host (string) e um dicionário com os dados daquele host (similar ao que seria obtido com `nm[host]` no modo síncrono).
- O método `still_scanning()` retorna `True` enquanto houver hosts sendo escaneados.
- `wait(segundos)` aguarda o tempo especificado, mas permite que callbacks sejam processados.

### Observações

- Não é possível acessar `nm[host]` diretamente no modo assíncrono; os resultados são entregues apenas via callback.
- Se você precisar de todos os resultados agregados, pode armazená‑los em uma lista/dicionário dentro da callback.
- O callback é executado na thread principal? Na implementação atual, o `PortScannerAsync` utiliza threads e a callback é chamada na thread principal (pois o `wait` libera a GIL e processa callbacks pendentes). Isso significa que você pode manipular dados compartilhados com segurança (desde que não faça operações bloqueantes longas dentro da callback).

---

## Argumentos Avançados do Nmap

O parâmetro `arguments` aceita qualquer opção de linha de comando do Nmap. Isso torna o módulo extremamente flexível. Alguns exemplos:

### Detecção de sistema operacional (`-O`)

```python
nm.scan('192.168.1.1', arguments='-O')
```

Após a varredura, as informações de SO podem ser acessadas em `nm[host]['osmatch']` (lista de possíveis sistemas) e `nm[host]['osclass']` (classificações).

### Detecção de versão de serviços (`-sV`)

```python
nm.scan('scanme.nmap.org', arguments='-sV')
```

Nas portas abertas, os campos `product`, `version` e `extrainfo` serão preenchidos.

### Scripts NSE (`-sC` ou `--script`)

```python
nm.scan('192.168.1.1', arguments='-sC')                # scripts padrão
nm.scan('192.168.1.1', arguments='--script=default')   # equivalente
nm.scan('192.168.1.1', arguments='--script=http-title') # script específico
```

Os resultados dos scripts são inseridos em `nm[host]['hostscript']` e, para scripts de porta, dentro do dicionário da porta na chave `'script'`.

### Varredura UDP (`-sU`)

```python
nm.scan('192.168.1.1', arguments='-sU -p 53,161')
```

Combine com `-sS` para varredura TCP e UDP ao mesmo tempo.

### Varredura agressiva (`-A`)

```python
nm.scan('192.168.1.1', arguments='-A')
```

Equivalente a `-O -sV -sC --traceroute`.

### Controle de tempo e paralelismo

```python
nm.scan('192.168.1.0/24', arguments='-T4 -min-hostgroup 64')
```

**Importante:** Sempre que usar argumentos que exigem privilégios elevados, execute o script como root.

---

## Tratamento de Erros e Exceções

O módulo pode lançar exceções em situações como:

- **`nmap.PortScannerError`**: erro geral relacionado ao Nmap (ex.: binário não encontrado, argumento inválido).
- **`socket.error`**: problemas de rede durante a varredura (raro, pois o Nmap trata internamente).
- **`KeyError`**: ao tentar acessar um host ou porta que não existe nos resultados.

É recomendável envolver as chamadas principais em blocos `try/except`.

```python
import nmap
import socket

nm = nmap.PortScanner()
try:
    nm.scan('scanme.nmap.org', '22-80')
except nmap.PortScannerError as e:
    print('Erro do Nmap:', e)
except socket.error as e:
    print('Erro de socket:', e)
except Exception as e:
    print('Erro inesperado:', e)

# Verifique se o host foi encontrado antes de acessar
if 'scanme.nmap.org' in nm.all_hosts():
    print(nm['scanme.nmap.org'].state())
```

---

## Dicas de Fluxo e Boas Práticas

### 1. **Verifique a disponibilidade do Nmap**
Antes de começar, confirme se o binário do Nmap está acessível:

```python
import nmap
nm = nmap.PortScanner()
if nm.nmap_version() == (0, 0):
    print("Nmap não encontrado. Verifique a instalação.")
else:
    print("Nmap versão", nm.nmap_version())
```

### 2. **Trate a ausência de resultados**
Sempre confira se o host está presente em `all_hosts()` antes de acessá‑lo.

### 3. **Use varreduras assíncronas para redes grandes**
Para evitar longas esperas bloqueantes, utilize `PortScannerAsync` com callbacks. Lembre‑se de manter o programa vivo até que todas as varreduras terminem.

### 4. **Evite repetir varreduras desnecessárias**
Se você precisar dos mesmos dados várias vezes, armazene os resultados em uma estrutura própria.

### 5. **Cuidado com permissões**
Varreduras como SYN stealth (`-sS`) e detecção de SO (`-O`) exigem privilégios de root. Execute o script com `sudo` ou configure capabilities no binário do Nmap.

### 6. **Documente os argumentos**
Ao usar argumentos complexos, deixe claro no código o que está sendo feito. Comentários ajudam na manutenção.

---

## Exemplos Práticos

### Exemplo 1: Varredura simples de uma rede local

```python
import nmap

nm = nmap.PortScanner()
rede = '192.168.1.0/24'
print(f'Varrendo rede {rede}...')
nm.scan(hosts=rede, arguments='-sn')   # -sn: ping scan (apenas hosts ativos)

print('Hosts ativos:')
for host in nm.all_hosts():
    if nm[host].state() == 'up':
        print(f'  {host} ({nm[host].hostname()})')
```

### Exemplo 2: Detecção de serviços e sistema operacional

```python
import nmap
import json

nm = nmap.PortScanner()
alvo = 'scanme.nmap.org'
print(f'Varredura completa em {alvo}...')
nm.scan(alvo, arguments='-O -sV')

for host in nm.all_hosts():
    print('-' * 50)
    print(f'Host: {host} ({nm[host].hostname()})')
    print(f'Estado: {nm[host].state()}')

    # Informações de SO
    if 'osmatch' in nm[host]:
        print('Possíveis sistemas operacionais:')
        for osm in nm[host]['osmatch']:
            print(f"  {osm['name']} (precisão: {osm['accuracy']}%)")

    # Portas abertas
    for proto in nm[host].all_protocols():
        portas = nm[host][proto].keys()
        for porta in sorted(portas):
            info = nm[host][proto][porta]
            if info['state'] == 'open':
                print(f'Porta {porta}/{proto} - {info["name"]} {info.get("version", "")}')
```

### Exemplo 3: Varredura assíncrona com script NSE

```python
import nmap

resultados = {}

def callback(host, scan_result):
    resultados[host] = scan_result
    print(f'[Concluído] {host} - {scan_result.get("status", {}).get("state", "unknown")}')

nm = nmap.PortScannerAsync()
nm.scan(hosts='192.168.1.1-10', arguments='--script http-title', callback=callback)

print('Aguardando conclusão das varreduras...')
while nm.still_scanning():
    nm.wait(2)

print('\nResumo dos resultados:')
for host, dados in resultados.items():
    print(f'{host}:')
    # Exemplo de extração de título HTTP se o script foi executado
    if 'tcp' in dados and 80 in dados['tcp'] and 'script' in dados['tcp'][80]:
        titulo = dados['tcp'][80]['script'].get('http-title', 'N/A')
        print(f'  Título HTTP na porta 80: {titulo}')
```

### Exemplo 4: Exportando resultados para CSV

```python
import csv
import nmap

nm = nmap.PortScanner()
nm.scan('192.168.1.1-5', arguments='-sV')

with open('resultados.csv', 'w', newline='') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(['Host', 'Porta', 'Protocolo', 'Estado', 'Serviço', 'Versão'])

    for host in nm.all_hosts():
        for proto in nm[host].all_protocols():
            portas = nm[host][proto].keys()
            for porta in sorted(portas):
                info = nm[host][proto][porta]
                writer.writerow([
                    host,
                    porta,
                    proto,
                    info['state'],
                    info['name'],
                    info.get('version', '')
                ])

print('Arquivo CSV gerado com sucesso.')
```

---

## Considerações Finais

O módulo `python-nmap` é uma ferramenta poderosa para integrar funcionalidades de varredura de rede em aplicações Python. Ele abstrai a complexidade da linha de comando e oferece uma interface orientada a objetos para análise dos resultados.

**Pontos fortes:**
- Fácil de usar, com métodos intuitivos.
- Suporte a varreduras síncronas e assíncronas.
- Acesso completo a todas as funcionalidades do Nmap através do parâmetro `arguments`.

**Limitações:**
- A performance em redes muito grandes pode ser melhor aproveitada com a versão assíncrona.
- Depende da instalação externa do Nmap.
- Algumas opções avançadas podem exigir tratamento específico dos resultados (ex.: scripts NSE).

