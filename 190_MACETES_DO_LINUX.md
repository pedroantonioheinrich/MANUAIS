```markdown
# 🐧 LINUX MACETES - O GUIA DEFINITIVO
### 100 Truques, Comandos e Gambiarras para Domar o Terminal

**Autor:** Assistente AI  
**Versão:** 2.0  
**Formato:** Markdown (.md)

---

## 📑 ÍNDICE RÁPIDO
1. [Navegação e Arquivos](#1-navegação-e-arquivos)
2. [Text Processing e Busca](#2-text-processing-e-busca)
3. [Permissões e Propriedade](#3-permissões-e-propriedade)
4. [Monitoramento do Sistema](#4-monitoramento-do-sistema)
5. [Gerenciamento de Processos](#5-gerenciamento-de-processos)
6. [Redes e Conectividade](#6-redes-e-conectividade)
7. [Pacotes e Atualizações](#7-pacotes-e-atualizações)
8. [Discos e Armazenamento](#8-discos-e-armazenamento)
9. [Segurança e Privacidade](#9-segurança-e-privacidade)
10. [Produtividade no Terminal](#10-produtividade-no-terminal)
11. [BÔNUS: Macetes de Performance](#11-bônus-macetes-de-performance)

---

## 1. NAVEGAÇÃO E ARQUIVOS

### #001 - Voltar para o Diretório Anterior Rapidamente
`cd -` = Volta para o último diretório onde você estava (como um "back" no terminal).

### #002 - Ver Árvore de Diretórios
`tree` = Mostra a estrutura de pastas em formato de árvore (instale com `apt install tree` se necessário).

### #003 - Criar Vários Diretórios de Uma Vez
`mkdir -p pasta1/pasta2/pasta3` = Cria toda a estrutura de uma só vez.

### #004 - Listar Arquivos com Tamanho Legível
`ls -lh` = Mostra tamanhos em KB, MB, GB em vez de bytes.

### #005 - Ver Arquivos Ocultos (Incluindo "." e "..")
`ls -la` = Lista tudo, incluindo diretórios `.` e `..`.

### #006 - Copiar Diretório Inteiro
`cp -r origem/ destino/` = Copia recursivamente pastas e subpastas.

### #007 - Mover Apenas Arquivos Específicos
`mv *.txt pasta/` = Move todos os .txt para a pasta destino.

### #008 - Remover Diretório com Conteúdo
`rm -rf pasta/` = Remove a pasta e tudo dentro (CUIDADO: não tem lixeira!).

### #009 - Criar Link Simbólico (Atalho)
`ln -s /caminho/origem /caminho/link` = Cria um atalho para o arquivo/pasta.

### #010 - Ver Tamanho de um Diretório
`du -sh pasta/` = Mostra o tamanho total da pasta de forma legível.

### #011 - Sincronizar Pastas (Backup Rápido)
`rsync -av origem/ destino/` = Sincroniza pastas mostrando progresso .

### #012 - Encontrar Arquivos por Nome
`find /caminho -name "arquivo.txt"` = Busca arquivos pelo nome exato .

### #013 - Encontrar Arquivos por Tipo
`find . -type f -name "*.pdf"` = Lista todos os PDFs na pasta atual .

### #014 - Encontrar Arquivos Modificados nos Últimos N Dias
`find . -mtime -7` = Arquivos modificados nos últimos 7 dias.

### #015 - Localizar Comando Rápido (Banco de Dados)
`locate arquivo.txt` = Busca mais rápida, mas usa banco de dados (atualize com `sudo updatedb`) .

### #016 - Saber Onde um Comando Está Instalado
`which comando` = Mostra o caminho do binário (ex: `which ls`).

### #017 - Ver Todos os Locais de um Comando
`whereis comando` = Mostra binário, fonte e páginas man .

### #018 - Criar Arquivo Vazio
`touch arquivo.txt` = Cria um arquivo vazio ou atualiza timestamp.

### #019 - Renomear Arquivos em Lote (Básico)
`rename 's/antigo/novo/' *.txt` = Substitui texto nos nomes dos arquivos.

### #020 - Mover 1000 Arquivos Aleatórios (xargs)
`find . -name "*.log" | xargs mv -t destino/` = Move todos os .log encontrados .

---

## 2. TEXT PROCESSING E BUSCA

### #021 - Buscar Texto Dentro de Arquivos
`grep "palavra" arquivo.txt` = Procura por "palavra" no arquivo .

### #022 - Busca Recursiva (Ignorar Case)
`grep -ri "erro" /var/log/` = Busca "erro" em todos os arquivos de log, ignorando maiúsculas.

### #023 - Mostrar Contexto da Busca
`grep -C 3 "falha" log.txt` = Mostra 3 linhas antes e depois do resultado.

### #024 - Contar Ocorrências
`grep -c "texto" arquivo.txt` = Mostra quantas linhas contêm o texto.

### #025 - Filtrar Colunas com AWK
`awk '{print $1, $3}' arquivo.txt` = Imprime a primeira e terceira coluna .

### #026 - Somar Valores com AWK
`awk '{sum += $1} END {print sum}' numeros.txt` = Soma todos os valores da primeira coluna.

### #027 - Substituir Texto com SED
`sed 's/antigo/novo/g' arquivo.txt` = Substitui todas as ocorrências .

### #028 - Editar Arquivo In-Place com SED
`sed -i 's/errado/certo/g' config.conf` = Altera o arquivo diretamente.

### #029 - Mostrar Início do Arquivo
`head -20 arquivo.txt` = Mostra as primeiras 20 linhas .

### #030 - Mostrar Fim do Arquivo (e Seguir)
`tail -f /var/log/syslog` = Mostra as últimas linhas e fica monitorando atualizações .

### #031 - Ordenar Linhas
`sort arquivo.txt` = Ordena alfabeticamente .

### #032 - Remover Linhas Duplicadas
`sort arquivo.txt | uniq` = Remove linhas repetidas (requer sort primeiro) .

### #033 - Contar Palavras, Linhas e Caracteres
`wc arquivo.txt` = Mostra linhas, palavras e bytes .

### #034 - Visualizar Arquivo com Paginação
`less arquivo_grande.log` = Navega com setas, busca com "/", sai com 'q' .

### #035 - Comparar Dois Arquivos
`diff arquivo1.txt arquivo2.txt` = Mostra as diferenças linha a linha .

### #036 - Mostrar Arquivo com Números de Linha
`cat -n arquivo.txt` = Exibe o conteúdo com numeração.

### #037 - Juntar Arquivos
`cat parte1.txt parte2.txt > completo.txt` = Concatena arquivos.

### #038 - Extrair Colunas com CUT
`cut -d: -f1 /etc/passwd` = Extrai o primeiro campo (usuários) usando ':' como delimitador .

### #039 - Processar JSON no Terminal
`echo '{"nome":"João"}' | jq .nome` = Requer `jq` instalado .

### #040 - Visualizar em Colunas
`column -t -s: /etc/passwd` = Formata saída em colunas alinhadas.

---

## 3. PERMISSÕES E PROPRIEDADE

### #041 - Tornar Script Executável
`chmod +x script.sh` = Adiciona permissão de execução .

### #042 - Permissões Numéricas (755, 644, etc)
`chmod 755 script.sh` = Dono: tudo, Grupo/Ler/exec, Outros: ler/exec.

### #043 - Mudar Dono de Arquivo
`chown usuario:grupo arquivo.txt` = Altera proprietário e grupo .

### #044 - Mudar Dono Recursivamente
`chown -R usuario:grupo pasta/` = Altera tudo dentro da pasta.

### #045 - Executar Comando como Outro Usuário
`sudo -u usuario comando` = Executa o comando como outro usuário.

### #046 - Ver Permissões em Formato Octal
`stat -c "%a %n" arquivo.txt` = Mostra permissões em número (ex: 644).

### #047 - Impedir Deleção de Arquivo (Imutável)
`sudo chattr +i arquivo.txt` = Torna o arquivo imutável (nem root deleta sem remover antes).

### #048 - Remover Imutabilidade
`sudo chattr -i arquivo.txt` = Remove o atributo imutável .

### #049 - Permitir Execução de Script para Todos
`chmod a+x script.sh` = 'a' = all (usuário, grupo, outros).

### #050 - Verificar Permissões de um Diretório
`namei -l /caminho/completo/para/pasta` = Mostra permissões de cada nível do caminho.

---

## 4. MONITORAMENTO DO SISTEMA

### #051 - Monitorar Processos em Tempo Real
`top` = Visualizador padrão de processos .

### #052 - Versão Melhorada do TOP
`htop` = Interface colorida, interativa (instale se necessário) .

### #053 - Ver Uso de Memória RAM
`free -h` = Mostra memória em formato humano (MB/GB) .

### #054 - Ver Uso de Disco
`df -h` = Mostra partições montadas e uso em formato humano .

### #055 - Ver Uso de Disco por Pasta
`du -sh *` = Tamanho de cada item na pasta atual .

### #056 - Quanto Tempo o PC Está Ligado?
`uptime` = Mostra tempo ligado e load average .

### #057 - Informações Detalhadas do Sistema
`uname -a` = Kernel, arquitetura, hostname .

### #058 - Versão da Distribuição
`lsb_release -a` ou `cat /etc/os-release` = Mostra qual Linux você está usando.

### #059 - Data e Hora
`date` = Mostra data atual .

### #060 - Calendário no Terminal
`cal 2025` = Mostra o calendário do ano .

### #061 - Estatísticas de CPU por Core
`mpstat -P ALL 1` = Atualiza a cada 1 segundo mostrando uso por core .

### #062 - Estatísticas de I/O
`iostat -x 1` = Mostra I/O de discos .

### #063 - Monitorar Arquivos Abertos por Processo
`lsof` = Lista arquivos abertos .

### #064 - Quem Está Logado?
`who` ou `w` = Mostra usuários conectados .

### #065 - Últimos Logins
`last` = Histórico de logins no sistema.

---

## 5. GERENCIAMENTO DE PROCESSOS

### #066 - Matar Processo por Nome
`pkill firefox` = Mata todos os processos com "firefox" no nome .

### #067 - Matar Processo por ID
`kill -9 1234` = Força a morte do processo com PID 1234 .

### #068 - Listar Processos de um Usuário
`ps -u usuario` = Processos pertencentes ao usuário .

### #069 - Ver Árvore de Processos
`pstree` = Mostra hierarquia de processos.

### #070 - Rodar Comando em Background
`comando &` = Executa e libera o terminal .

### #071 - Trazer Processo para Foreground
`fg` = Traz o último job background para frente .

### #072 - Listar Jobs em Background
`jobs` = Mostra processos em background no terminal atual .

### #073 - Prioridade de Processo (Nice)
`nice -n 10 comando` = Executa com prioridade baixa .

### #074 - Mudar Prioridade de Processo Rodando
`renice +5 -p 1234` = Aumenta o nice (diminui prioridade) do PID 1234 .

### #075 - Ignorar Hangup (Rodar após Logout)
`nohup comando &` = Continua rodando mesmo após fechar terminal .

---

## 6. REDES E CONECTIVIDADE

### #076 - Ver IP Local
`ip a` ou `ifconfig` = Mostra interfaces de rede .

### #077 - Testar Conectividade
`ping -c 4 google.com` = Envia 4 pacotes e para .

### #078 - Ver Portas Abertas
`ss -tulpn` = Socket statistics, moderno e rápido .

### #079 - Versão Antiga mas Ainda Usada
`netstat -tulpn` = Mostra serviços escutando .

### #080 - Download de Arquivos
`wget https://exemplo.com/arquivo.iso` = Baixa arquivo da internet .

### #081 - Testar APIs/URLs
`curl -I https://google.com` = Mostra apenas cabeçalhos HTTP .

### #082 - Ver Rota até um Host
`traceroute google.com` = Mostra cada salto até o destino .

### #083 - DNS Lookup
`dig google.com` = Consulta detalhada de DNS .

### #084 - DNS Simples
`nslookup google.com` = Consulta DNS tradicional .

### #085 - Escanear Portas com Netcat
`nc -zv 192.168.1.1 1-1000` = Verifica portas abertas no IP.

### #086 - Capturar Pacotes de Rede
`sudo tcpdump -i eth0` = Sniffer de rede no terminal .

### #087 - Transferir Arquivo via Netcat
Receptor: `nc -l -p 1234 > arquivo.txt`  
Emissor: `nc IP_DO_RECEPTOR 1234 < arquivo.txt`

### #088 - Verificar Largura de Banda
`iftop` = Monitora largura de banda em tempo real .

### #089 - SSH com Chave (Sem Senha)
`ssh-copy-id usuario@servidor` = Copia chave pública para o servidor .

### #090 - Montar Pasta Remota via SSH
`sshfs usuario@servidor:/pasta /pasta_local` = Requer `sshfs` instalado.

---

## 7. PACOTES E ATUALIZAÇÕES

### #091 - Atualizar Lista de Pacotes (Debian/Ubuntu)
`sudo apt update` = Atualiza índices .

### #092 - Atualizar Todos os Pacotes
`sudo apt upgrade -y` = Atualiza tudo sem perguntar .

### #093 - Instalar Pacote
`sudo apt install nome_do_pacote` = Instala programa .

### #094 - Remover Pacote
`sudo apt remove nome_do_pacote` = Desinstala .

### #095 - Procurar Pacote
`apt search palavra_chave` = Busca pacotes disponíveis .

### #096 - Limpar Cache de Pacotes
`sudo apt autoremove && sudo apt autoclean` = Remove pacotes órfãos e limpa cache .

### #097 - Para Red Hat/CentOS/Fedora (YUM/DNF)
`sudo dnf install nome_do_pacote` = Instala no Fedora/RHEL .

### #098 - Ver Pacotes Instalados
`dpkg -l` (Debian) ou `rpm -qa` (Red Hat) = Lista tudo instalado .

### #099 - Ver Informações de um Pacote
`apt show nome_do_pacote` = Mostra detalhes, dependências, etc.

### #100 - Consertar Pacotes Quebrados
`sudo apt --fix-broken install` = Corrige dependências quebradas.

---

## 8. DISCOS E ARMAZENAMENTO

### #101 - Montar Disco/Partição
`sudo mount /dev/sdb1 /mnt` = Monta a partição em /mnt .

### #102 - Desmontar Disco
`sudo umount /mnt` = Desmonta (cuidado: não pode estar em uso) .

### #103 - Ver Discos e Partições
`lsblk` = Lista blocos (discos/partições) em formato de árvore.

### #104 - Ver UUIDs dos Discos
`blkid` = Mostra identificadores únicos das partições .

### #105 - Verificar Espaço em Disco (Inodes)
`df -i` = Mostra uso de inodes (útil se disco cheio mas `df` mostra espaço livre).

### #106 - Criar Sistema de Arquivos (Formatar)
`sudo mkfs.ext4 /dev/sdb1` = Formata a partição como ext4 .

### #107 - Compactar Pasta com TAR.GZ
`tar -czvf arquivo.tar.gz pasta/` = Compacta .

### #108 - Descompactar TAR.GZ
`tar -xzvf arquivo.tar.gz` = Extrai .

### #109 - Compactar com ZIP
`zip -r arquivo.zip pasta/` = Compacta em formato ZIP .

### #110 - Descompactar ZIP
`unzip arquivo.zip` = Extrai ZIP .

### #111 - Ver Conteúdo do Arquivo sem Extrair
`tar -tvf arquivo.tar.gz` = Lista conteúdo do .tar.gz.

### #112 - Compactar com BZIP2 (Maior Compressão)
`tar -cjvf arquivo.tar.bz2 pasta/` = Mais lento, mas comprime mais.

### #113 - Criar Imagem ISO de Disco/CD
`dd if=/dev/cdrom of=imagem.iso` = Cria ISO do CD/DVD .

### #114 - Queimar ISO no Pendrive (CUIDADO!)
`sudo dd if=imagem.iso of=/dev/sdX bs=4M status=progress` = Substitua sdX pelo dispositivo correto!

### #115 - Verificar SMART do Disco
`sudo smartctl -a /dev/sda` = Mostra saúde do HD/SSD .

---

## 9. SEGURANÇA E PRIVACIDADE

### #116 - Mudar Senha do Usuário
`passwd` = Altera sua senha .

### #117 - Bloquear Usuário
`sudo passwd -l nome_usuario` = Bloqueia login do usuário.

### #118 - Ver Logs de Autenticação
`sudo tail -f /var/log/auth.log` = Monitora tentativas de login .

### #119 - Firewall Simples (UFW)
`sudo ufw enable` = Ativa firewall básico .

### #120 - Permitir Porta no Firewall
`sudo ufw allow 22/tcp` = Libera SSH.

### #121 - Escanear Arquivos com ClamAV
`clamscan -r /home/usuario` = Verifica vírus na pasta home.

### #122 - Criptografar Arquivo com GPG
`gpg -c arquivo.txt` = Criptografia simétrica com senha .

### #123 - Descriptografar GPG
`gpg arquivo.txt.gpg` = Restaura arquivo original.

### #124 - Ver Conexões Estabelecidas
`ss -tup` = Mostra conexões ativas com programas .

### #125 - Remover Arquivos com Segurança (Sobrescrever)
`shred -uvz arquivo.txt` = Sobrescreve, remove e zera o espaço .

### #126 - Verificar Integridade de Pacotes
`debsums -s` = Verifica se arquivos de pacotes foram alterados (Debian/Ubuntu).

### #127 - Bloquear IP no Firewall
`sudo iptables -A INPUT -s 192.168.1.100 -j DROP` = Bloqueia IP específico.

### #128 - 2FA no Login do Linux
`sudo apt install libpam-google-authenticator` = Adiciona autenticação de dois fatores .

### #129 - Remover Arquivos Temporários Antigos
`sudo find /tmp -type f -atime +10 -delete` = Deleta arquivos não acessados há 10+ dias .

### #130 - Limpar Logs Antigos
`sudo journalctl --vacuum-time=2weeks` = Mantém apenas logs das últimas 2 semanas .

---

## 10. PRODUTIVIDADE NO TERMINAL

### #131 - Repetir Último Comando
`!!` = Executa novamente o comando anterior .

### #132 - Repetir Último Argumento
`!$` = Usa o último argumento do comando anterior .

### #133 - Executar Comando Específico do Histórico
`!123` = Executa o comando número 123 do `history` .

### #134 - Busca Reversa no Histórico
`Ctrl + R` = Digite parte do comando para buscar .

### #135 - Ver Histórico de Comandos
`history` = Lista últimos comandos executados .

### #136 - Limpar Terminal
`Ctrl + L` = Limpa a tela (mesmo que `clear`).

### #137 - Cancelar Comando Atual
`Ctrl + C` = Interrompe o processo em execução .

### #138 - Sair do Terminal
`Ctrl + D` = Fecha a sessão (logout) .

### #139 - Pular Palavra no Terminal
`Alt + F` = Vai para próxima palavra  
`Alt + B` = Volta para palavra anterior.

### #140 - Ir para Início/Fim da Linha
`Ctrl + A` = Início da linha  
`Ctrl + E` = Fim da linha.

### #141 - Apagar Linha Inteira
`Ctrl + U` = Apaga tudo antes do cursor.

### #142 - Apagar até o Fim da Linha
`Ctrl + K` = Apaga do cursor até o fim.

### #143 - Criar Alias (Atalho) Temporário
`alias ll='ls -la'` = Cria comando personalizado .

### #144 - Criar Alias Permanente
Adicione no `~/.bashrc` ou `~/.zshrc`: `alias atualizar='sudo apt update && sudo apt upgrade'`

### #145 - Encadeamento de Comandos (E)
`comando1 && comando2` = Executa comando2 apenas se comando1 funcionar .

### #146 - Encadeamento de Comandos (OU)
`comando1 || comando2` = Executa comando2 apenas se comando1 falhar .

### #147 - Encadeamento Independente
`comando1; comando2` = Executa ambos, independente de sucesso/falha .

### #148 - Pipe (Conectar Comandos)
`comando1 | comando2` = Saída de 1 vira entrada de 2 .

### #149 - Redirecionar Saída para Arquivo
`comando > arquivo.txt` = Sobrescreve o arquivo com a saída .

### #150 - Redirecionar Saída (Adicionar)
`comando >> arquivo.txt` = Adiciona ao final do arquivo .

### #151 - Redirecionar Erros
`comando 2> erros.txt` = Apenas mensagens de erro vão para o arquivo.

### #152 - Executar Comando Editando (fc)
`fc` = Abre último comando no editor para modificar .

### #153 - Corrigir Comando Anterior (^)
`^errado^certo` = Corrige erro no comando anterior e executa .

### #154 - Abrir Editor para Comando Longo
`Ctrl + X + E` = Abre o comando atual no editor .

### #155 - Monitorar Comando Repetidamente
`watch -n 5 comando` = Executa comando a cada 5 segundos .

---

## 11. BÔNUS: MACETES DE PERFORMANCE

### #156 - Governador de CPU (Performance vs Economia)
`echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor` = Máximo desempenho .

### #157 - Ajustar Swappiness (Uso de Swap)
`sudo sysctl vm.swappiness=10` = Usa swap apenas quando necessário .

### #158 - Tornar Swappiness Permanente
Adicione `vm.swappiness=10` no `/etc/sysctl.conf`.

### #159 - ZRAM (Swap Comprimido na RAM)
`sudo apt install zram-config` = Cria swap comprimido na RAM (mais rápido que disco) .

### #160 - I/O Scheduler para SSD
`echo none | sudo tee /sys/block/sda/queue/scheduler` = Melhor para SSDs (noop ou none) .

### #161 - Limpar Cache de Memória
`sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches` = Libera caches (não matar processos).

### #162 - Ver Transparent HugePages
`cat /sys/kernel/mm/transparent_hugepage/enabled` = Geralmente já ativo .

### #163 - Limitar CPU de um Processo (CPULimit)
`sudo cpulimit -p 1234 -l 50` = Limita o processo 1234 a 50% de CPU.

### #164 - Ver Temperatura da CPU
`sensors` = Requer `lm-sensors` instalado e configurado.

### #165 - Ver Frequência da CPU em Tempo Real
`watch -n 1 "cat /proc/cpuinfo | grep MHz"` = Monitora frequência de cada core.

### #166 - Desativar Acesso à Swap
`sudo swapoff -a` = Desliga swap (se tiver RAM suficiente).

### #167 - Ativar Swap Novamente
`sudo swapon -a` = Reativa swap.

### #168 - Ver I/O em Tempo Real
`iotop` = Mostra processos por I/O de disco.

### #169 - Ver Processos por Uso de Memória
`ps aux --sort=-%mem | head` = Top 10 processos que mais usam RAM.

### #170 - Ver Processos por Uso de CPU
`ps aux --sort=-%cpu | head` = Top 10 processos que mais usam CPU.

---

## 🎯 COMANDOS SALVA-VIDAS (EMERGÊNCIA)

### #171 - Sistema Não Inicia? Acessar Modo Recovery
Na inicialização, segure Shift (GRUB) > Advanced > Recovery mode > Root.

### #172 - Esqueceu a Senha do Root/Usuário
No recovery mode root: `passwd usuario` ou `passwd root` .

### #173 - Tela Congelada? Mata Tudo
`Ctrl + Alt + F2` (ou F3-F6) = Muda para terminal virtual, mata o processo e volta com `Ctrl + Alt + F1`.

### #174 - Socorro! Apaguei Arquivo Importante
`sudo extundelete /dev/sda1 --restore-file /caminho/arquivo` = Tenta recuperar (se não sobrescrito).

### #175 - Sistema Lento? Verificar Load
`uptime` e `top` = Load average > número de cores significa sobrecarga.

### #176 - Matar Processo que Trava o Sistema
`Ctrl + Alt + Del` = Pode reiniciar (depende da configuração).

### #177 - Desligar Imediatamente
`sudo shutdown -h now` ou `sudo poweroff` .

### #178 - Reiniciar Imediatamente
`sudo reboot` ou `sudo shutdown -r now` .

### #179 - Parar Serviço Problemático
`sudo systemctl stop nome_do_servico` .

### #180 - Ver Erros de Kernel
`dmesg | tail -20` = Últimos erros do kernel .

---

## 📚 MANIPULAÇÃO AVANÇADA

### #181 - Criar Arquivo com Conteúdo Imediato
`cat > arquivo.txt << EOF` + digita + `EOF` = Cria arquivo com múltiplas linhas.

### #182 - Dividir Arquivo Grande em Partes
`split -b 100M arquivo_grande.iso parte_` = Divide em partes de 100MB.

### #183 - Juntar Partes de Arquivo
`cat parte_* > arquivo_grande.iso` = Reconstitui o arquivo original.

### #184 - Diff com Contexto Colorido
`diff -u arquivo1 arquivo2 | colordiff` = Requer `colordiff` instalado.

### #185 - Ver Espaço por Extensão
`find . -type f -name "*.mp4" -exec du -ch {} + | grep total$` = Total de espaço usado por MP4s.

### #186 - Encontrar Arquivos Duplicados
`fdupes -r .` = Requer `fdupes` instalado.

### #187 - Backup Incremental com Rsync
`rsync -av --backup --backup-dir=/backup/$(date +%Y%m%d) /origem/ /destino/` = Backup com histórico.

### #188 - Sincronizar com Servidor Remoto
`rsync -avz -e ssh /pasta_local usuario@servidor:/pasta_remota` = Sincroniza via SSH.

### #189 - Executar Comando em Vários Servidores
`for servidor in server1 server2; do ssh $servidor "comando"; done` = Loop para múltiplos hosts.

### #190 - Manter Processo Rodando (Mesmo se fechar terminal)
`screen` ou `tmux` = Multiplexadores de terminal .

---

## ⚠️ AVISOS IMPORTANTES

1. **Comandos com `sudo`** : Podem danificar o sistema se usados incorretamente. Sempre entenda o que está fazendo.
2. **`rm -rf`** : Não use como root a menos que tenha certeza absoluta. Não há lixeira no terminal!
3. **Discos (`/dev/sda`, etc)** : Tenha certeza do dispositivo antes de formatar ou usar `dd`.
4. **Backup** : Antes de fazer tweaks no kernel ou alterar configurações do sistema, faça backup.
5. **Distribuições**: Comandos de pacotes (`apt`, `dnf`, `pacman`) variam conforme a distro.

---
