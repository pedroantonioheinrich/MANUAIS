# Manual Completo de Comandos Terminal - Linux Mint

## Índice
1. [Introdução](#introdução)
2. [Navegação no Sistema de Arquivos](#navegação-no-sistema-de-arquivos)
3. [Manipulação de Arquivos e Diretórios](#manipulação-de-arquivos-e-diretórios)
4. [Permissões e Propriedade](#permissões-e-propriedade)
5. [Processos e Sistema](#processos-e-sistema)
6. [Rede](#rede)
7. [Pacotes e Instalação](#pacotes-e-instalação)
8. [Editores de Texto](#editores-de-texto)
9. [Busca e Filtragem](#busca-e-filtragem)
10. [Compactação e Descompactação](#compactação-e-descompactação)
11. [Atalhos do Terminal](#atalhos-do-terminal)
12. [Variáveis de Ambiente](#variáveis-de-ambiente)
13. [Scripts e Automação](#scripts-e-automação)

---

## Introdução

### Terminal vs Console
- **Terminal**: Interface gráfica que emula um console
- **Console**: Interface de texto direto com o sistema

### Acessando o Terminal
- `Ctrl + Alt + T`: Atalho padrão para abrir terminal
- Menu → Terminal ou Konsole dependendo do ambiente

### Sintaxe Básica
```
comando [opções] [argumentos]
```
- **Comando**: Ação a ser executada
- **Opções**: Modificam o comportamento (geralmente começam com `-` ou `--`)
- **Argumentos**: Alvo da ação

### Ajuda e Documentação
```bash
man comando          # Manual do comando
comando --help       # Ajuda rápida
info comando         # Informações detalhadas
whatis comando       # Breve descrição
```

---

## Navegação no Sistema de Arquivos

### Comandos Básicos
```bash
pwd                  # Mostra diretório atual (Print Working Directory)
ls                   # Lista arquivos e diretórios
cd diretorio         # Muda para diretório especificado
cd ~                 # Vai para o diretório home
cd -                 # Volta para diretório anterior
cd ..                # Sobe um nível no diretório
```

### Opções Comuns do `ls`
```bash
ls -l                # Lista detalhada (permissões, tamanho, data)
ls -a                # Mostra arquivos ocultos (começam com .)
ls -lh               # Lista com tamanhos legíveis (KB, MB)
ls -la               # Combina -l e -a
ls -t                # Ordena por data (mais recente primeiro)
ls -r                # Ordena reversamente
ls -R                # Lista recursivamente subdiretórios
```

### Árvore de Diretórios
```bash
tree                 # Mostra estrutura em árvore (instalar: sudo apt install tree)
tree -L 2            # Limita a 2 níveis de profundidade
tree -d              # Mostra apenas diretórios
```

---

## Manipulação de Arquivos e Diretórios

### Criar Arquivos e Diretórios
```bash
touch arquivo.txt    # Cria arquivo vazio ou atualiza timestamp
mkdir diretorio      # Cria diretório
mkdir -p dir1/dir2   # Cria diretórios com pais (parents)
mkdir -m 755 dir     # Cria com permissões específicas
```

### Copiar, Mover e Renomear
```bash
cp origem destino    # Copia arquivo/diretório
cp -r dir1 dir2      # Copia recursivamente (para diretórios)
cp -v arquivo* dest  # Copia com modo verbose (mostra o que faz)

mv origem destino    # Move ou renomeia arquivo/diretório
mv -i antigo novo    # Modo interativo (pergunta antes de sobrescrever)

rename 's/\.htm$/\.html/' *.htm  # Renomeia extensões
```

### Remover Arquivos e Diretórios
```bash
rm arquivo           # Remove arquivo
rm -i arquivo        # Remove com confirmação interativa
rm -f arquivo        # Remove forçadamente (force)
rm -r diretorio      # Remove diretório e conteúdo recursivamente
rm -rf diretorio     # Remove forçadamente e recursivamente (CUIDADO!)

rmdir diretorio      # Remove diretório vazio
```

### Visualizar Arquivos
```bash
cat arquivo          # Mostra conteúdo completo
less arquivo         # Mostra página por página (navegação: espaço, q)
more arquivo         # Similar ao less (menos recursos)
head arquivo         # Mostra primeiras 10 linhas
head -n 20 arquivo   # Mostra primeiras 20 linhas
tail arquivo         # Mostra últimas 10 linhas
tail -n 15 arquivo   # Mostra últimas 15 linhas
tail -f arquivo      # Segue arquivo (atualiza conforme cresce)
```

### Comparar Arquivos
```bash
diff arquivo1 arquivo2  # Mostra diferenças entre arquivos
cmp arquivo1 arquivo2   # Compara arquivos binários
```

---

## Permissões e Propriedade

### Entendendo Permissões
```
drwxr-xr-x 1 user group
↑ ↑  ↑  ↑
| |  |  |
| |  |  Outros: r-x (ler e executar)
| |  Grupo: r-x (ler e executar)
| Usuário: rwx (ler, escrever, executar)
Tipo: d=diretório, -=arquivo, l=link
```

### Modificar Permissões
```bash
chmod u+x arquivo    # Adiciona permissão de execução para usuário
chmod g-w arquivo    # Remove permissão de escrita para grupo
chmod o=r arquivo    # Define outros como somente leitura
chmod 755 arquivo    # rwxr-xr-x (usuário: tudo, grupo/outros: ler/executar)
chmod 644 arquivo    # rw-r--r-- (usuário: ler/escrever, outros: ler)
chmod -R 755 dir     # Aplica recursivamente
```

### Modificar Propriedade
```bash
chown usuario arquivo        # Muda dono do arquivo
chown usuario:grupo arquivo  # Muda dono e grupo
chown -R usuario diretorio   # Aplica recursivamente
chgrp grupo arquivo          # Muda apenas o grupo
```

### Permissões Especiais
```bash
chmod +s arquivo     # Seta bit SUID (executa como dono)
chmod +t diretorio   # Seta sticky bit (apenas dono pode deletar)
```

---

## Processos e Sistema

### Visualizar Processos
```bash
ps                    # Lista processos do terminal atual
ps aux                # Lista todos os processos do sistema
ps -ef                # Lista todos os processos (formato completo)
top                   # Monitora processos em tempo real
htop                  # Top melhorado (instalar: sudo apt install htop)
pstree                # Mostra processos em árvore
```

### Gerenciar Processos
```bash
kill PID              # Envia sinal TERM (terminar) para processo
kill -9 PID           # Envia sinal KILL (matar imediatamente)
killall nome          # Mata todos processos com nome
pkill nome            # Mata processos por nome
nice -n 10 comando    # Executa comando com prioridade baixa
renice 5 PID          # Altera prioridade de processo existente
```

### Informações do Sistema
```bash
uname -a              # Informações do kernel
hostname              # Nome do host
whoami                # Usuário atual
id                    # Informações do usuário (UID, GID, grupos)
uptime                # Tempo ligado e carga média
free -h               # Memória livre (em formato legível)
df -h                 # Espaço em disco (human readable)
du -sh diretorio      # Tamanho do diretório
lscpu                 # Informações da CPU
lsblk                 # Lista dispositivos de bloco
```

### Reiniciar e Desligar
```bash
sudo reboot           # Reinicia o sistema
sudo shutdown -h now  # Desliga agora
sudo shutdown -r +5   # Reinicia em 5 minutos
sudo poweroff         # Desliga o sistema
```

---

## Rede

### Informações de Rede
```bash
ip addr show          # Mostra interfaces e IPs (substitui ifconfig)
ip route show         # Mostra tabela de roteamento
ss -tulpn             # Portas abertas e processos (substitui netstat)
ping host             # Testa conectividade
traceroute host       # Traça rota até host
host dominio.com      # Consulta DNS
dig dominio.com       # Consulta DNS detalhada
nslookup dominio.com  # Consulta DNS (legado)
```

### Transferência de Arquivos
```bash
wget URL              # Baixa arquivo da web
curl URL              # Transfere dados com mais opções
scp arquivo user@host:destino  # Copia via SSH
rsync -av origem destino       # Sincroniza arquivos
```

### Conexões SSH
```bash
ssh user@host         # Conecta via SSH
ssh -p porta user@host # Conecta em porta específica
ssh-keygen            # Gera chaves SSH
ssh-copy-id user@host # Copia chave pública para host
```

### Firewall
```bash
sudo ufw status       # Status do firewall
sudo ufw enable       # Ativa firewall
sudo ufw disable      # Desativa firewall
sudo ufw allow 22     # Permite porta 22 (SSH)
sudo ufw deny 80      # Bloqueia porta 80
```

---

## Pacotes e Instalação

### APT (Advanced Package Tool)
```bash
sudo apt update       # Atualiza lista de pacotes
sudo apt upgrade      # Atualiza pacotes instalados
sudo apt full-upgrade # Atualiza com remoção de pacotes se necessário
sudo apt install pacote # Instala pacote
sudo apt remove pacote  # Remove pacote
sudo apt purge pacote   # Remove com configurações
sudo apt autoremove     # Remove dependências não usadas
sudo apt search termo   # Busca pacotes
sudo apt show pacote    # Mostra informações do pacote
apt list --installed   # Lista pacotes instalados
```

### Gerenciamento de Repositórios
```bash
sudo add-apt-repository ppa:usuario/ppa  # Adiciona PPA
sudo apt-key add chave.gpg               # Adiciona chave GPG
sudo apt edit-sources                    # Edita sources.list
```

### Pacotes .deb
```bash
sudo dpkg -i pacote.deb    # Instala pacote .deb
sudo dpkg -r pacote        # Remove pacote .deb
sudo dpkg -l               # Lista pacotes instalados
```

### Snap e Flatpak
```bash
sudo snap install pacote   # Instala snap
sudo snap remove pacote    # Remove snap
flatpak install pacote     # Instala flatpak
flatpak remove pacote      # Remove flatpak
```

---

## Editores de Texto

### Nano (Simples)
```bash
nano arquivo.txt          # Abre arquivo no nano
# Atalhos do Nano:
# Ctrl + O: Salvar
# Ctrl + X: Sair
# Ctrl + K: Cortar linha
# Ctrl + U: Colar
# Ctrl + W: Buscar
```

### Vim (Avançado)
```bash
vim arquivo.txt           # Abre arquivo no vim
# Modos do Vim:
# i: Modo inserção
# Esc: Modo normal
# :w: Salvar
# :q: Sair
# :q!: Sair sem salvar
# :wq: Salvar e sair
# /texto: Buscar
```

---

## Busca e Filtragem

### Buscar Arquivos
```bash
find . -name "*.txt"          # Encontra arquivos .txt no diretório atual
find /home -user usuario      # Encontra arquivos por dono
find . -size +10M             # Encontra arquivos maiores que 10MB
find . -mtime -7              # Encontra modificados nos últimos 7 dias
find . -type f -perm 644      # Encontra arquivos com permissão 644
find . -exec chmod 644 {} \;  # Executa comando nos resultados
```

### Buscar Conteúdo
```bash
grep "texto" arquivo          # Busca texto em arquivo
grep -r "texto" diretorio     # Busca recursivamente
grep -i "texto" arquivo       # Case insensitive
grep -v "texto" arquivo       # Inverte busca (linhas sem o texto)
grep -n "texto" arquivo       # Mostra números das linhas
grep -E "padrão" arquivo      # Expressão regular estendida
```

### Filtrar e Processar Texto
```bash
cat arquivo | grep "texto"    # Pipeline: passa saída como entrada
cat arquivo | sort            # Ordena linhas
cat arquivo | uniq            # Remove duplicados consecutivos
cat arquivo | wc -l           # Conta linhas
cat arquivo | head -20        # Primeiras 20 linhas
cat arquivo | tail -30        # Últimas 30 linhas
cat arquivo | cut -d':' -f1   # Corta primeira colona separada por :
cat arquivo | awk '{print $1}' # Imprime primeira coluna
```

---

## Compactação e Descompactação

### Tar (Tape Archive)
```bash
tar -cvf arquivo.tar diretorio    # Cria tar
tar -xvf arquivo.tar              # Extrai tar
tar -tvf arquivo.tar              # Lista conteúdo
tar -czvf arquivo.tar.gz dir      # Cria tar.gz (compactado gzip)
tar -xzvf arquivo.tar.gz          # Extrai tar.gz
tar -cjvf arquivo.tar.bz2 dir     # Cria tar.bz2 (bzip2)
tar -xjvf arquivo.tar.bz2         # Extrai tar.bz2
```

### Zip
```bash
zip -r arquivo.zip diretorio      # Cria zip
unzip arquivo.zip                 # Extrai zip
unzip -l arquivo.zip              # Lista conteúdo
```

### Gzip e Bzip2
```bash
gzip arquivo                      # Compacta para .gz
gunzip arquivo.gz                 # Descompacta .gz
bzip2 arquivo                     # Compacta para .bz2
bunzip2 arquivo.bz2               # Descompacta .bz2
```

---

## Atalhos do Terminal

### Navegação na Linha de Comando
```
Ctrl + A          # Vai para início da linha
Ctrl + E          # Vai para fim da linha
Ctrl + U          # Apaga do cursor até início
Ctrl + K          # Apaga do cursor até fim
Ctrl + W          # Apaga palavra anterior
Ctrl + Y          # Cola texto apagado
Ctrl + R          # Busca no histórico
Ctrl + C          # Interrompe comando atual
Ctrl + D          # Envia EOF (fecha terminal se linha vazia)
Ctrl + Z          # Suspende processo (fg para retornar)
Ctrl + L          # Limpa tela (igual a clear)
```

### Abas e Janelas
```
Ctrl + Shift + T  # Nova aba
Ctrl + Shift + W  # Fecha aba
Ctrl + PageUp     # Próxima aba
Ctrl + PageDown   # Aba anterior
Ctrl + Shift + N  # Nova janela
Ctrl + Shift + Q  # Fecha janela
```

---

## Variáveis de Ambiente

### Visualizar e Definir
```bash
echo $PATH           # Mostra variável PATH
env                  # Lista todas variáveis de ambiente
set                  # Lista variáveis de shell
export VAR=valor     # Define variável de ambiente
unset VAR            # Remove variável
```

### Variáveis Importantes
```bash
$HOME                # Diretório home do usuário
$USER                # Nome do usuário atual
$SHELL               # Shell atual
$PWD                 # Diretório atual
$PATH                # Caminhos para executáveis
$PS1                 # Prompt de comando
```

### Adicionar ao PATH
```bash
export PATH=$PATH:/novo/caminho        # Temporário
# Adicionar ao ~/.bashrc para permanente:
echo 'export PATH=$PATH:/novo/caminho' >> ~/.bashrc
source ~/.bashrc                       # Recarrega configurações
```

---

## Scripts e Automação

### Criar Script Básico
```bash
#!/bin/bash
# Este é um comentário
echo "Olá, Mundo!"
```

### Permissões e Execução
```bash
chmod +x script.sh    # Torna executável
./script.sh           # Executa script
bash script.sh        # Executa com bash
```

### Variáveis em Scripts
```bash
#!/bin/bash
VARIAVEL="valor"
echo "Valor: $VARIAVEL"
```

### Entrada do Usuário
```bash
#!/bin/bash
echo "Digite seu nome:"
read NOME
echo "Olá, $NOME!"
```

### Condicionais
```bash
#!/bin/bash
if [ $1 -gt 10 ]; then
    echo "Maior que 10"
elif [ $1 -eq 10 ]; then
    echo "Igual a 10"
else
    echo "Menor que 10"
fi
```

### Loops
```bash
#!/bin/bash
# For loop
for i in {1..5}; do
    echo "Número: $i"
done

# While loop
CONTADOR=0
while [ $CONTADOR -lt 5 ]; do
    echo "Contador: $CONTADOR"
    CONTADOR=$((CONTADOR+1))
done
```

### Funções
```bash
#!/bin/bash
saudacao() {
    echo "Olá, $1!"
}

saudacao "Mundo"
```

---

## Dicas e Boas Práticas

### 1. Use Tab Completion
- Pressione Tab para auto-completar comandos, arquivos e diretórios
- Tab duas vezes mostra opções disponíveis

### 2. Histórico de Comandos
```bash
history              # Mostra histórico
!n                   # Executa comando número n do histórico
!!                   # Repete último comando
!string              # Executa último comando começando com string
Ctrl + R             # Busca reversa no histórico
```

### 3. Redirecionamento
```bash
comando > arquivo    # Redireciona saída para arquivo (sobrescreve)
comando >> arquivo   # Redireciona saída para arquivo (anexa)
comando < arquivo    # Redireciona entrada de arquivo
comando 2> erro.log  # Redireciona erros
comando &> saida.log # Redireciona saída e erros
```

### 4. Execução em Background
```bash
comando &            # Executa em background
jobs                 # Lista jobs em background
fg %1                # Traz job 1 para foreground
bg %1                # Coloca job 1 em background
```

### 5. Aliases (Apelidos)
```bash
alias ll='ls -la'    # Cria alias
unalias ll           # Remove alias
# Adicionar ao ~/.bashrc para permanente
```

### 6. Scripts de Inicialização
- `~/.bashrc`: Executado para shells interativos não-login
- `~/.bash_profile`: Executado para shells login
- `~/.profile`: Alternativa ao .bash_profile

### 7. Segurança
- Evite executar comandos como root sem necessidade
- Use `sudo` para comandos que exigem privilégios
- Verifique comandos antes de executar, especialmente com `rm -rf`

---

## Recursos Adicionais

### Documentação Oficial
- `man hier`: Hierarquia do sistema de arquivos
- `man bash`: Manual completo do bash
- `man sudoers`: Configuração do sudo

### Sites de Referência
- [Linux Mint Documentation](https://linuxmint.com/documentation.php)
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
- [TLDR Pages](https://tldr.sh/): Exemplos práticos de comandos

### Próximos Passos
1. Personalize seu prompt com `PS1`
2. Crie aliases para comandos frequentes
3. Automatize tarefas com scripts
4. Explore ferramentas avançadas como `awk`, `sed`, `regex`
5. Aprenda sobre `cron` para agendamento de tarefas

---

*Este manual foi criado para Linux Mint, mas a maioria dos comandos funciona em qualquer distribuição Linux. Sempre consulte `man comando` para informações específicas e atualizadas.*
