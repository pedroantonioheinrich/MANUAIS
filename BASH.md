---
# 📘 Manual Completo de BASH (Linux)
**Do Básico à Automação Avançada**

---

## 1. 🚀 Introdução: O que é BASH?

**BASH** = **B**ourne **A**gain **SH**ell. É o interpretador de comandos mais comum no Linux. Funciona como uma ponte entre você e o kernel do sistema.

### Por que aprender BASH?
- Automatizar tarefas repetitivas
- Controle total sobre o sistema
- Base para administração de servidores

---

## 2. 🏁 Primeiros Passos

### 2.1 O Terminal
Abra o terminal (Ctrl+Alt+T no Ubuntu). Você verá algo como:
```
usuario@computador:~$
```
- `~` significa seu diretório **home**
- `$` indica usuário comum (`#` seria root)

### 2.2 Primeiro Comando: `echo`
```bash
echo "Olá, mundo!"
```
Exibe texto no terminal. Efeito colateral: Nenhum, é seguro.

### 2.3 Navegação Básica
```bash
pwd                    # Mostra onde você está (print working directory)
ls                     # Lista arquivos
ls -la                 # Lista tudo (incluindo ocultos) com detalhes
cd /home               # Vai para o diretório /home
cd ~                   # Volta para seu home
cd ..                  # Sobe um nível
```

### 2.4 Manipulação de Arquivos
```bash
touch arquivo.txt      # Cria arquivo vazio
mkdir pasta            # Cria diretório
cp arquivo.txt backup/ # Copia arquivo para pasta backup
mv arquivo.txt novo.txt# Renomeia ou move
rm arquivo.txt         # Remove arquivo (⚠️ CUIDADO: não vai para lixeira!)
rm -rf pasta/          # Remove pasta e tudo dentro (🔥 MUITO CUIDADO!)
```

**Efeitos colaterais do `rm -rf`**:  
- Remove permanentemente (não recuperável por métodos simples)
- Com `sudo rm -rf /` você destrói o sistema operacional (NUNCA faça!)

---

## 3. 📂 Trabalhando com Arquivos e Texto

### 3.1 Visualizando Arquivos
```bash
cat arquivo.txt        # Mostra conteúdo completo
less arquivo.txt       # Navega com setas (q para sair)
head -n 5 arquivo.txt  # Primeiras 5 linhas
tail -f log.txt        # Mostra últimas linhas e atualiza em tempo real
```

### 3.2 Redirecionamentos e Pipes
```bash
echo "texto" > arquivo.txt    # Sobrescreve arquivo
echo "mais" >> arquivo.txt    # Adiciona ao final
ls -la | grep ".txt"          # Lista e filtra apenas .txt
```

**Efeitos colaterais do `>`**:  
- Se o arquivo existir, será sobrescrito sem aviso!

### 3.3 Filtros Poderosos
```bash
grep "erro" log.txt           # Busca linhas com "erro"
grep -i "erro" log.txt        # Case insensitive
grep -r "função" ./           # Busca recursiva em todos arquivos

sort nomes.txt                 # Ordena linhas
uniq                           # Remove duplicatas (geralmente usado com sort)
wc -l arquivo.txt              # Conta linhas
```

---

## 4. 🔧 Permissões e Usuários

### 4.1 Entendendo Permissões
```bash
ls -l arquivo.txt
# -rw-r--r-- 1 usuario grupo 1024 Jan 1 10:00 arquivo.txt
```
- `-` = arquivo comum (`d` = diretório)
- `rw-` = dono pode ler/escrever
- `r--` = grupo pode ler
- `r--` = outros podem ler

### 4.2 Modificando Permissões
```bash
chmod +x script.sh        # Torna executável
chmod 755 script.sh       # Modo octal: dono=rwx, grupo=rx, outros=rx
chown usuario:arquivo.txt # Muda dono
```

**Efeitos colaterais**:  
- Permissões muito abertas (`777`) são risco de segurança
- Remover execução de scripts pode quebrar programas

---

## 5. ⚙️ Variáveis e Ambiente

### 5.1 Variáveis Simples
```bash
NOME="João"
echo $NOME                # João
echo "Meu nome é $NOME"   # Meu nome é João
```

### 5.2 Variáveis de Ambiente
```bash
echo $PATH                # Onde o sistema procura comandos
export MINHA_VAR="valor"  # Torna disponível para programas filhos
env                       # Lista todas variáveis de ambiente
```

### 5.3 Variáveis Especiais
```bash
$?                        # Código de saída do último comando (0 = sucesso)
$$                        # PID do processo atual
$HOME                     # Seu diretório home
```

---

## 6. 🤖 Scripts Básicos

### 6.1 Primeiro Script
Crie `meuscript.sh`:
```bash
#!/bin/bash
# Isso é um comentário
echo "Iniciando script..."
nome="Maria"
echo "Olá, $nome"
```

Torne executável e execute:
```bash
chmod +x meuscript.sh
./meuscript.sh
```

### 6.2 Parâmetros
```bash
#!/bin/bash
echo "Primeiro parâmetro: $1"
echo "Todos parâmetros: $@"
echo "Número de parâmetros: $#"
```

Execute:
```bash
./meuscript.sh arg1 arg2 arg3
```

---

## 7. 🔀 Controle de Fluxo

### 7.1 Condicionais (`if`)
```bash
#!/bin/bash
if [ -f "$1" ]; then
    echo "$1 é um arquivo"
elif [ -d "$1" ]; then
    echo "$1 é um diretório"
else
    echo "$1 não existe"
fi
```

**Testes comuns**:
- `-f` = arquivo existe
- `-d` = diretório existe
- `-z` = string vazia
- `-n` = string não vazia
- `=` = strings iguais
- `-eq` = números iguais

### 7.2 Loops

**`for`**:
```bash
for i in 1 2 3; do
    echo "Número $i"
done

for arquivo in *.txt; do
    echo "Processando $arquivo"
done
```

**`while`**:
```bash
contador=1
while [ $contador -le 5 ]; do
    echo "Contagem: $contador"
    ((contador++))
done
```

### 7.3 `case` (múltiplas escolhas)
```bash
case "$1" in
    start)
        echo "Iniciando..."
        ;;
    stop)
        echo "Parando..."
        ;;
    *)
        echo "Use start ou stop"
        ;;
esac
```

---

## 8. 📦 Funções

```bash
saudacao() {
    local nome="$1"        # local = variável local
    echo "Olá, $nome!"
}

saudacao "Ana"              # Olá, Ana!
```

---

## 9. 🧠 Tópicos Intermediários

### 9.1 Substituição de Comandos
```bash
data=$(date +%Y-%m-%d)
echo "Hoje é $data"

arquivos=$(ls | wc -l)
echo "Total: $arquivos arquivos"
```

### 9.2 Arrays
```bash
frutas=("maçã" "banana" "laranja")
echo ${frutas[0]}           # maçã
echo ${frutas[@]}           # todas
echo ${#frutas[@]}          # tamanho
```

### 9.3 Expressões Aritméticas
```bash
a=5
b=3
soma=$((a + b))
echo $soma                   # 8

# Ou com let
let "resultado = a * b"
echo $resultado              # 15
```

### 9.4 Expansão de Variáveis
```bash
nome="João Silva"
echo ${nome:0:4}             # João (posição 0, 4 caracteres)
echo ${nome/Silva/Souza}     # João Souza
echo ${nome^^}               # JOÃO SILVA (maiúsculas)
```

---

## 10. 🔥 Avançado: Automação e Boas Práticas

### 10.1 Tratamento de Erros
```bash
#!/bin/bash
set -e                       # Sai se qualquer comando falhar
set -u                       # Erro se variável não definida
set -o pipefail              # Falha se qualquer parte do pipe falhar

# Trap para capturar Ctrl+C
trap 'echo "Script interrompido"; exit 1' INT
```

### 10.2 Processamento em Lote
```bash
#!/bin/bash
# Backup de todos .txt com timestamp
backup_dir="backup_$(date +%Y%m%d_%H%M%S)"
mkdir "$backup_dir"

for arquivo in *.txt; do
    cp "$arquivo" "$backup_dir/${arquivo%.txt}.bak"
    echo "✅ $arquivo copiado"
done

tar -czf "$backup_dir.tar.gz" "$backup_dir"
rm -rf "$backup_dir"
echo "🎉 Backup concluído: $backup_dir.tar.gz"
```

### 10.3 Interagindo com Usuário
```bash
read -p "Digite seu nome: " nome
read -s -p "Digite sua senha: " senha   # -s = silent
echo
echo "Olá, $nome"

# Menu interativo
select opcao in "Listar" "Sair"; do
    case $opcao in
        "Listar") ls ;;
        "Sair") break ;;
        *) echo "Opção inválida" ;;
    esac
done
```

### 10.4 Trabalhando com Arquivos CSV
```bash
#!/bin/bash
# Processa dados.csv: nome,idade,cidade
while IFS=',' read -r nome idade cidade; do
    echo "Nome: $nome, Idade: $idade, Cidade: $cidade"
    if [ $idade -gt 60 ]; then
        echo "  → $nome é idoso"
    fi
done < dados.csv
```

### 10.5 Paralelismo Básico
```bash
# Executa comandos em background
comando1 &
comando2 &
comando3 &
wait                        # Aguarda todos terminarem
echo "Todos finalizados!"
```

---

## 11. ⚠️ Efeitos Colaterais e Cuidados

### Perigos Comuns
| Comando | Efeito Colateral | Prevenção |
|---------|------------------|-----------|
| `rm -rf /` | Destroi sistema | **NUNCA** faça |
| `chmod 777` | Permissões inseguras | Use `755` para scripts |
| `>` | Sobrescreve sem aviso | Use `>>` para adicionar |
| `sudo` | Executa como root | Use só quando necessário |
| Variáveis não definidas | Comportamento inesperado | Use `set -u` |

### Debug
```bash
bash -x script.sh          # Mostra cada comando executado
set -x                      # Dentro do script, ativa debug
set +x                      # Desativa debug
```

---

## 12. 🎯 Projeto Prático: Monitor de Sistema

Crie `monitor.sh`:
```bash
#!/bin/bash
# Monitor simples do sistema

while true; do
    clear
    echo "📊 MONITOR DO SISTEMA - $(date)"
    echo "══════════════════════════════════"
    
    echo "🔹 CPU:"
    top -bn1 | grep "Cpu(s)" | awk '{print "   Uso: " $2 "%"}'
    
    echo -e "\n🔹 MEMÓRIA:"
    free -h | grep Mem | awk '{print "   Total: " $2 " | Usada: " $3 " | Livre: " $4}'
    
    echo -e "\n🔹 DISCO:"
    df -h / | grep / | awk '{print "   Total: " $2 " | Usada: " $3 " | Disp.: " $4}'
    
    echo -e "\n🔹 PROCESSOS:"
    ps aux --sort=-%cpu | head -4
    
    sleep 5
done
```

---


**Lembre-se**: Com grandes poderes vêm grandes responsabilidades. Sempre faça backup e teste scripts em ambiente seguro primeiro!

Bons scripts! 🐧
