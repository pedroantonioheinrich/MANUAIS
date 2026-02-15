# Manual: Como Criar um Executável de Programa Python

## Índice
1. [Introdução](#introdução)
2. [Pré-requisitos](#pré-requisitos)
3. [Método 1: PyInstaller](#método-1-pyinstaller)
4. [Método 2: cx_Freeze](#método-2-cx_freeze)
5. [Método 3: Py2exe](#método-3-py2exe)
6. [Dicas e Soluções de Problemas](#dicas-e-soluções-de-problemas)
7. [Exemplo Prático](#exemplo-prático)

## Introdução

Este manual explica como converter programas Python em arquivos executáveis independentes (.exe no Windows, executáveis em Linux/Mac). Isso permite distribuir sua aplicação para usuários que não têm Python instalado.

## Pré-requisitos

- Python instalado (versão 3.6 ou superior)
- Pip (gerenciador de pacotes Python)
- Conhecimento básico de linha de comando
- Seu programa Python funcionando corretamente

## Método 1: PyInstaller

### Instalação
```bash
pip install pyinstaller
```

### Comandos Básicos

**Executável simples:**
```bash
pyinstaller meu_programa.py
```

**Executável com uma única pasta:**
```bash
pyinstaller --onefile meu_programa.py
```

**Executável sem console (para aplicações GUI):**
```bash
pyinstaller --onefile --noconsole meu_programa.py
```

**Com ícone personalizado:**
```bash
pyinstaller --onefile --icon=meu_icone.ico meu_programa.py
```

### Exemplo Completo
```bash
pyinstaller --onefile --windowed --name="MeuApp" --icon=app.ico meu_programa.py
```

### Arquivo de Configuração (.spec)
Crie um arquivo `.spec` para configurações avançadas:

```python
# meu_programa.spec
a = Analysis(['meu_programa.py'],
             pathex=['/caminho/do/projeto'],
             binaries=[],
             datas=[('pasta_dados/*', 'pasta_dados')],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=None,
             noarchive=False)

pyz = PYZ(a.pure, a.zipped_data, cipher=None)

exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          [],
          name='MeuApp',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          upx_exclude=[],
          runtime_tmpdir=None,
          console=False,
          icon='app.ico')
```

## Método 2: cx_Freeze

### Instalação
```bash
pip install cx-freeze
```

### Arquivo setup.py
```python
# setup.py
from cx_Freeze import setup, Executable

# Dependências
build_exe_options = {
    "packages": ["os", "sys", "tkinter"],  # Adicione os pacotes necessários
    "excludes": ["tkinter.test"],  # Exclua pacotes desnecessários
    "include_files": ["pasta_de_recursos/"]  # Inclua arquivos adicionais
}

# Configuração
setup(
    name="MeuApp",
    version="1.0",
    description="Descrição do meu aplicativo",
    options={"build_exe": build_exe_options},
    executables=[Executable("meu_programa.py", base="Win32GUI" if GUI else None)]
)
```

### Gerar Executável
```bash
python setup.py build
```

## Método 3: Py2exe

### Instalação
```bash
pip install py2exe
```

### Arquivo setup.py
```python
# setup.py
from distutils.core import setup
import py2exe
import sys

# Para aplicações GUI, use isso
if len(sys.argv) == 1:
    sys.argv.append("py2exe")
    sys.argv.append("-q")

setup(
    windows=[{"script": "meu_programa.py"}],  # Para GUI
    # console=[{"script": "meu_programa.py"}],  # Para console
    options={
        "py2exe": {
            "includes": ["sip"],  # Pacotes adicionais
            "excludes": ["_ssl"],  # Excluir pacotes
            "bundle_files": 1,  # 1 = executável único
            "compressed": True,
            "optimize": 2
        }
    },
    zipfile=None,  # Incluir bibliotecas no executável
)
```

## Dicas e Soluções de Problemas

### Redução de Tamanho do Executável
```bash
# Usar UPX para compressão
# Baixe UPX de https://upx.github.io/
# Coloque upx.exe no PATH ou na pasta do projeto
pyinstaller --onefile --upx-dir=caminho/para/upx meu_programa.py
```

### Lidando com Arquivos de Dados
```python
# PyInstaller - incluir arquivos adicionais
pyinstaller --onefile --add-data "pasta_recursos;recursos" meu_programa.py
```

### Erros Comuns e Soluções

1. **Módulo não encontrado:**
   ```bash
   pyinstaller --hidden-import=nome_do_modulo meu_programa.py
   ```

2. **Erro de caminho de arquivos:**
   ```python
   import sys
   import os
   
   def resource_path(relative_path):
       """Obter caminho absoluto para recursos"""
       if hasattr(sys, '_MEIPASS'):
           return os.path.join(sys._MEIPASS, relative_path)
       return os.path.join(os.path.abspath("."), relative_path)
   
   # Usar assim:
   arquivo = resource_path("dados/config.txt")
   ```

3. **Erro de DLL faltando:**
   ```bash
   # Instalar Microsoft Visual C++ Redistributable
   # https://aka.ms/vs/17/release/vc_redist.x64.exe
   ```

## Exemplo Prático

### Programa de Exemplo (calculadora.py)
```python
# calculadora.py
import tkinter as tk
from tkinter import messagebox

def calcular():
    try:
        num1 = float(entry1.get())
        num2 = float(entry2.get())
        operacao = operacao_var.get()
        
        if operacao == "+":
            resultado = num1 + num2
        elif operacao == "-":
            resultado = num1 - num2
        elif operacao == "*":
            resultado = num1 * num2
        elif operacao == "/":
            if num2 == 0:
                messagebox.showerror("Erro", "Divisão por zero!")
                return
            resultado = num1 / num2
        
        label_resultado.config(text=f"Resultado: {resultado}")
    except ValueError:
        messagebox.showerror("Erro", "Digite números válidos!")

# Interface gráfica
janela = tk.Tk()
janela.title("Calculadora")
janela.geometry("300x250")

tk.Label(janela, text="Número 1:").pack()
entry1 = tk.Entry(janela)
entry1.pack()

tk.Label(janela, text="Número 2:").pack()
entry2 = tk.Entry(janela)
entry2.pack()

tk.Label(janela, text="Operação:").pack()
operacao_var = tk.StringVar(value="+")
frame_operacoes = tk.Frame(janela)
frame_operacoes.pack()

for op in ["+", "-", "*", "/"]:
    tk.Radiobutton(frame_operacoes, text=op, variable=operacao_var, value=op).pack(side=tk.LEFT)

tk.Button(janela, text="Calcular", command=calcular).pack(pady=10)

label_resultado = tk.Label(janela, text="Resultado: ")
label_resultado.pack()

janela.mainloop()
```

### Gerando o Executável com PyInstaller
```bash
# Instalar PyInstaller
pip install pyinstaller

# Criar executável com ícone
pyinstaller --onefile --windowed --name="Calculadora" calculadora.py

# O executável estará em dist/Calculadora.exe
```

### Testando
1. Navegue até a pasta `dist`
2. Execute o arquivo `Calculadora.exe`
3. Verifique se tudo funciona corretamente

## Conclusão

Agora você tem as ferramentas necessárias para criar executáveis de seus programas Python. Recomendo começar com o PyInstaller por sua simplicidade e recursos. Lembre-se de sempre testar o executável em uma máquina sem Python instalado para garantir que todas as dependências foram incluídas corretamente.

### Checklist Final
- [ ] Programa testado e funcionando
- [ ] Todas as dependências instaladas
- [ ] Ícone personalizado (opcional)
- [ ] Arquivos de dados incluídos
- [ ] Executável testado em máquina limpa
- [ ] Tamanho do arquivo verificado
- [ ] Documentação básica criada
