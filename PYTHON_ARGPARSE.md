# Manual Completo do Módulo `argparse` em Python

O módulo `argparse` é a ferramenta padrão do Python para criar interfaces de linha de comando (CLI) de forma elegante e robusta. Ele permite definir argumentos, opções, subcomandos, gerar mensagens de ajuda automaticamente e tratar erros de entrada. Este manual cobre desde o básico até tópicos avançados, com exemplos práticos.

---

## Índice

1. [Introdução](#introdução)
2. [Estrutura básica](#estrutura-básica)
3. [Argumentos posicionais](#argumentos-posicionais)
4. [Argumentos opcionais](#argumentos-opcionais)
5. [Tipos de dados](#tipos-de-dados)
6. [Ações (`action`)](#ações-action)
7. [Múltiplos valores (`nargs`)](#múltiplos-valores-nargs)
8. [Valores padrão](#valores-padrão)
9. [Restrição de escolhas (`choices`)](#restrição-de-escolhas-choices)
10. [Mensagens de ajuda](#mensagens-de-ajuda)
11. [Argumentos mutuamente exclusivos](#argumentos-mutuamente-exclusivos)
12. [Subcomandos (`subparsers`)](#subcomandos-subparsers)
13. [Argumentos de arquivo (`argparse.FileType`)](#argumentos-de-arquivo-argparsefiletype)
14. [Personalização da exibição de ajuda](#personalização-da-exibição-de-ajuda)
15. [Tratamento de erros e validação customizada](#tratamento-de-erros-e-validação-customizada)
16. [Boas práticas](#boas-práticas)
17. [Exemplo completo](#exemplo-completo)
18. [Referências](#referências)

---

## Introdução

`argparse` substitui os antigos `getopt` e `optparse`. Ele analisa automaticamente `sys.argv`, gera mensagens de ajuda e trata entradas inválidas. Para usá‑lo, basta importar o módulo, criar um objeto `ArgumentParser` e adicionar argumentos.

---

## Estrutura básica

```python
import argparse

parser = argparse.ArgumentParser(description='Descrição do programa.')
parser.add_argument('arg1', help='descrição do argumento')
args = parser.parse_args()

print(args.arg1)
```

O método `parse_args()` retorna um objeto com os argumentos como atributos (nomeados conforme definido).

---

## Argumentos posicionais

Argumentos posicionais são obrigatórios por padrão e sua ordem importa.

```python
parser.add_argument('nome', help='nome do usuário')
parser.add_argument('idade', type=int, help='idade em anos')
args = parser.parse_args()

print(f'{args.nome} tem {args.idade} anos.')
```

Execução:
```
$ python script.py João 30
João tem 30 anos.
```

---

## Argumentos opcionais

Argumentos opcionais começam com `-` ou `--`. Por padrão, não são obrigatórios e armazenam o valor seguinte.

```python
parser.add_argument('-v', '--verbose', action='store_true', help='modo verboso')
parser.add_argument('-o', '--output', help='arquivo de saída')
args = parser.parse_args()
```

- `action='store_true'` armazena `True` se a opção for usada.
- Para opções com valor, o padrão é `action='store'` (implícito).

---

## Tipos de dados

Use `type` para converter a entrada automaticamente. Tipos comuns: `int`, `float`, `str` (padrão). Também aceita funções customizadas.

```python
parser.add_argument('--taxa', type=float, default=0.1)
parser.add_argument('--data', type=lambda d: d.upper())
```

---

## Ações (`action`)

A ação define como o argumento é manipulado:

| Ação               | Descrição                                          |
|--------------------|----------------------------------------------------|
| `'store'`          | Armazena o valor (padrão).                         |
| `'store_const'`    | Armazena um valor constante definido por `const`.  |
| `'store_true'`     | Armazena `True` se presente.                       |
| `'store_false'`    | Armazena `False` se presente.                      |
| `'append'`         | Acumula valores (pode repetir a opção).            |
| `'append_const'`   | Acumula um valor constante.                        |
| `'count'`          | Conta quantas vezes a opção apareceu.              |
| `'help'`           | Exibe ajuda e sai.                                 |
| `'version'`        | Exibe versão e sai.                                 |

Exemplo:

```python
parser.add_argument('-q', '--quiet', action='store_true')
parser.add_argument('-d', '--debug', action='store_const', const=True, default=False)
parser.add_argument('-v', action='count', default=0)
parser.add_argument('--item', action='append')
```

---

## Múltiplos valores (`nargs`)

Controla quantos argumentos um parâmetro consome:

- `nargs=N` : exatamente N argumentos (lista).
- `nargs='?'` : zero ou um argumento.
- `nargs='*'` : zero ou mais (lista).
- `nargs='+'` : um ou mais (lista).
- `nargs=argparse.REMAINDER` : todo o restante da linha.

```python
parser.add_argument('arquivos', nargs='+', help='arquivos a processar')
parser.add_argument('--limite', nargs=2, type=int, metavar=('MIN', 'MAX'))
```

---

## Valores padrão

Use `default` para definir um valor quando o argumento não for fornecido.

```python
parser.add_argument('--porta', type=int, default=8080)
```

Se a ação for `'store_true'`, o padrão é `False`; para `'store_false'`, o padrão é `True`.

---

## Restrição de escolhas (`choices`)

Limita os valores aceitos a um conjunto pré‑definido.

```python
parser.add_argument('--modo', choices=['rápido', 'lento'], default='rápido')
```

---

## Mensagens de ajuda

O `argparse` gera ajuda automaticamente com `-h` ou `--help`. Para personalizar:

- `description` no construtor (texto antes da lista de argumentos).
- `epilog` (texto após a lista).
- `help` em cada argumento.
- `metavar` para substituir o nome do argumento na ajuda.

Exemplo:

```python
parser = argparse.ArgumentParser(
    description='Faz algo incrível.',
    epilog='Veja mais em https://exemplo.com'
)
parser.add_argument('--arquivo', metavar='CAMINHO', help='caminho do arquivo')
```

Para desabilitar a opção `-h`, use `add_help=False` no construtor.

---

## Argumentos mutuamente exclusivos

Grupos de argumentos que não podem ser usados juntos.

```python
grupo = parser.add_mutually_exclusive_group()
grupo.add_argument('-v', '--verbose', action='store_true')
grupo.add_argument('-q', '--quiet', action='store_true')
```

Também é possível torná‑los obrigatórios (`required=True` no grupo).

---

## Subcomandos (`subparsers`)

Ideal para programas com múltiplos comandos (como `git commit`, `git push`).

```python
parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest='comando', required=True, help='comandos disponíveis')

# Subcomando "iniciar"
parser_iniciar = subparsers.add_parser('iniciar', help='inicia o serviço')
parser_iniciar.add_argument('--host', default='localhost')
parser_iniciar.add_argument('--porta', type=int, default=80)

# Subcomando "parar"
parser_parar = subparsers.add_parser('parar', help='para o serviço')
parser_parar.add_argument('--force', action='store_true')

args = parser.parse_args()
if args.comando == 'iniciar':
    print(f'Iniciando em {args.host}:{args.porta}')
elif args.comando == 'parar':
    print('Parando... (forçado?', args.force, ')')
```

---

## Argumentos de arquivo (`argparse.FileType`)

Facilita a abertura de arquivos com tratamento automático de erros.

```python
parser.add_argument('--entrada', type=argparse.FileType('r'), required=True)
parser.add_argument('--saida', type=argparse.FileType('w'))
args = parser.parse_args()

conteudo = args.entrada.read()
args.saida.write(conteudo)   # se args.saida existir
args.entrada.close()
```

O arquivo é aberto e será fechado automaticamente ao final do programa (se usar `with` ou fechar explicitamente).

---

## Personalização da exibição de ajuda

É possível substituir o formatador de ajuda:

- `argparse.RawDescriptionHelpFormatter`: preserva quebras de linha na descrição.
- `argparse.RawTextHelpFormatter`: preserva quebras de linha em todos os textos de ajuda.
- `argparse.ArgumentDefaultsHelpFormatter`: exibe valores padrão na ajuda.
- `argparse.MetavarTypeHelpFormatter`: mostra o tipo em vez do nome do argumento.

Exemplo:

```python
parser = argparse.ArgumentParser(
    formatter_class=argparse.ArgumentDefaultsHelpFormatter
)
```

---

## Tratamento de erros e validação customizada

Por padrão, `parse_args()` sai do programa em caso de erro. Para capturar exceções:

```python
try:
    args = parser.parse_args()
except argparse.ArgumentError as e:
    print('Erro:', e)
    # tratamento manual
```

Também é possível adicionar validações personalizadas após o `parse_args()`:

```python
args = parser.parse_args()
if args.idade < 0:
    parser.error('idade não pode ser negativa')
```

---

## Boas práticas

1. **Use nomes claros** para opções longas (`--nome-completo`).
2. **Defina `help` para todos os argumentos** para gerar ajuda útil.
3. **Prefira opções com valor a argumentos posicionais** quando a ordem não for óbvia.
4. **Use `type` para converter** e validar tipos.
5. **Evite ações complexas**; prefira validar após o parsing.
6. **Documente o comportamento** na descrição e epílogo.
7. **Utilize `argparse.FileType`** para lidar com arquivos de forma segura.
8. **Agrupe argumentos relacionados** com `add_argument_group`.

---

## Exemplo completo

```python
#!/usr/bin/env python3
# processador.py
import argparse

def main():
    parser = argparse.ArgumentParser(
        description='Processador de textos',
        epilog='Desenvolvido por Fulano',
        formatter_class=argparse.ArgumentDefaultsHelpFormatter
    )
    parser.add_argument('arquivo', type=argparse.FileType('r'),
                        help='arquivo de entrada')
    parser.add_argument('--saida', type=argparse.FileType('w'),
                        default='saida.txt',
                        help='arquivo de saída')
    parser.add_argument('--maiusculas', action='store_true',
                        help='converter para maiúsculas')
    parser.add_argument('--contar-linhas', action='store_true',
                        help='exibir contagem de linhas')
    parser.add_argument('-v', '--verbose', action='count', default=0,
                        help='aumenta verbosidade (-v, -vv)')
    
    args = parser.parse_args()
    
    if args.verbose >= 1:
        print(f'Lendo {args.arquivo.name}...')
    
    texto = args.arquivo.read()
    args.arquivo.close()
    
    if args.maiusculas:
        texto = texto.upper()
    
    if args.contar_linhas:
        linhas = texto.count('\n')
        print(f'Número de linhas: {linhas}')
    
    args.saida.write(texto)
    args.saida.close()
    
    if args.verbose >= 2:
        print('Processamento concluído.')

if __name__ == '__main__':
    main()
```

---

## Referências

- Documentação oficial: [argparse — Parser for command-line options](https://docs.python.org/pt-br/3/library/argparse.html)
- Tutorial oficial: [Argparse Tutorial](https://docs.python.org/pt-br/3/howto/argparse.html)
- PEP 389: argparse – New Command Line Parsing Module

---
