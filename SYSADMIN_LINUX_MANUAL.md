# Manual Completo para Formação de SysAdmin Linux Mint

## Índice
1. [Introdução](#introdução)
2. [Pré-requisitos](#pré-requisitos)
3. [Módulo 1 - Fundamentos do Linux](#módulo-1---fundamentos-do-linux)
4. [Módulo 2 - Instalação e Configuração](#módulo-2---instalação-e-configuração)
5. [Módulo 3 - Sistema de Arquivos](#módulo-3---sistema-de-arquivos)
6. [Módulo 4 - Gerenciamento de Pacotes](#módulo-4---gerenciamento-de-pacotes)
7. [Módulo 5 - Gerenciamento de Usuários e Permissões](#módulo-5---gerenciamento-de-usuários-e-permissões)
8. [Módulo 6 - Processos e Serviços](#módulo-6---processos-e-serviços)
9. [Módulo 7 - Rede e Conectividade](#módulo-7---rede-e-conectividade)
10. [Módulo 8 - Segurança Básica](#módulo-8---segurança-básica)
11. [Módulo 9 - Scripts e Automação](#módulo-9---scripts-e-automação)
12. [Módulo 10 - Monitoramento e Troubleshooting](#módulo-10---monitoramento-e-troubleshooting)
13. [Módulo 11 - Serviços de Rede](#módulo-11---serviços-de-rede)
14. [Módulo 12 - Virtualização e Containers](#módulo-12---virtualização-e-containers)
15. [Projetos Práticos](#projetos-práticos)
16. [Recursos Adicionais](#recursos-adicionais)
17. [Certificações Recomendadas](#certificações-recomendadas)

## Introdução

Este manual tem como objetivo fornecer um caminho estruturado para se tornar um Administrador de Sistemas (SysAdmin) especializado em Linux Mint. O Linux Mint é uma distribuição Linux baseada no Ubuntu, que por sua vez é baseada no Debian, sendo amplamente utilizada em ambientes desktop e servidores.

## Pré-requisitos

- Conhecimento básico de informática
- Acesso a um computador para instalação do Linux Mint
- Conexão com internet
- 10-15 horas semanais de estudo
- Vontade de aprender e experimentar

## Módulo 1 - Fundamentos do Linux

### 1.1 História e Filosofia do Linux
- Origem do Linux (Linus Torvalds)
- Distribuições Linux
- Filosofia Unix/Linux
- Software Livre vs Open Source

### 1.2 Terminal e Linha de Comando
```bash
# Comandos básicos
pwd, ls, cd, mkdir, rmdir, touch, cp, mv, rm
cat, less, more, head, tail
nano, vim (básico)

# Atalhos importantes
Ctrl+C, Ctrl+Z, Ctrl+D, Ctrl+L
Tab (auto-completar)
Ctrl+R (pesquisar histórico)
```

### 1.3 Estrutura do Sistema de Arquivos
```
/           # Raiz do sistema
├── bin     # Binários essenciais
├── boot    # Arquivos de boot
├── etc     # Arquivos de configuração
├── home    # Diretórios dos usuários
├── lib     # Bibliotecas essenciais
├── opt     # Software adicional
├── root    # Diretório do root
├── sbin    # Binários do sistema
├── tmp     # Arquivos temporários
├── usr     # Recursos do usuário
└── var     # Arquivos variáveis
```

## Módulo 2 - Instalação e Configuração

### 2.1 Preparação para Instalação
- Download da ISO do Linux Mint
- Criação de mídia bootável (USB/DVD)
- Verificação de integridade (checksum)
- Backup de dados existentes

### 2.2 Processo de Instalação
- Particionamento manual vs automático
- Configuração de LVM (opcional)
- Configuração de swap
- Instalação de codecs e drivers
- Configuração de usuário e senha

### 2.3 Pós-instalação
```bash
# Atualização do sistema
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade

# Instalação de ferramentas essenciais
sudo apt install build-essential git curl wget net-tools
```

### 2.4 Gerenciamento de Boot
- GRUB2 configuração
- Timeshift (sistema de snapshot)
- Gerenciamento de kernels
- Modo de recuperação

## Módulo 3 - Sistema de Arquivos

### 3.1 Sistemas de Arquivos Suportados
- ext4 (padrão)
- Btrfs (avançado)
- XFS
- NTFS/FAT32 (compatibilidade)

### 3.2 Comandos Avançados
```bash
# Manipulação de arquivos
find, locate, whereis, which
grep, awk, sed
tar, gzip, bzip2, xz

# Permissões e propriedade
chmod, chown, chgrp
umask, stat

# Montagem de sistemas de arquivos
mount, umount
/etc/fstab configuração
```

### 3.3 Monitoramento de Espaço
```bash
df -h                 # Espaço livre
du -sh *              # Uso por diretório
ncdu                  # Interface ncurses
baobab                # Interface gráfica
```

## Módulo 4 - Gerenciamento de Pacotes

### 4.1 Sistema APT
```bash
# Comandos essenciais
sudo apt update
sudo apt upgrade
sudo apt install <pacote>
sudo apt remove <pacote>
sudo apt purge <pacote>
sudo apt autoremove
sudo apt search <termo>
apt-cache show <pacote>

# Gerenciamento de repositórios
/etc/apt/sources.list
/etc/apt/sources.list.d/
sudo apt-add-repository
```

### 4.2 Outros Formatos
- DEB (nativo)
- Flatpak
- Snap (opcional)
- AppImage

### 4.3 Compilação de Software
```bash
./configure
make
sudo make install
```

### 4.4 Gerenciamento de Dependências
```bash
apt-cache depends <pacote>
apt-cache rdepends <pacote>
sudo apt-get build-dep <pacote>
```

## Módulo 5 - Gerenciamento de Usuários e Permissões

### 5.1 Usuários e Grupos
```bash
# Criação e gestão
sudo adduser <usuario>
sudo deluser <usuario>
sudo usermod -aG <grupo> <usuario>
sudo groupadd <grupo>
sudo groupdel <grupo>

# Arquivos de configuração
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
```

### 5.2 Permissões Linux
```bash
# Permissões básicas (rwx)
chmod 755 arquivo      # rwxr-xr-x
chmod 644 arquivo      # rw-r--r--

# Permissões especiais
setuid, setgid, sticky bit
chmod u+s, chmod g+s, chmod +t
```

### 5.3 Controle de Acesso
- sudoers configuration
- Polkit (PolicyKit)
- PAM (Pluggable Authentication Modules)

## Módulo 6 - Processos e Serviços

### 6.1 Gerenciamento de Processos
```bash
# Visualização
ps aux
top, htop
pstree

# Controle
kill, killall, pkill
nice, renice
bg, fg, jobs
```

### 6.2 Systemd
```bash
# Comandos básicos
sudo systemctl start <servico>
sudo systemctl stop <servico>
sudo systemctl restart <servico>
sudo systemctl reload <servico>
sudo systemctl enable <servico>
sudo systemctl disable <servico>
sudo systemctl status <servico>

# Logs
sudo journalctl -f
sudo journalctl -u <servico>
```

### 6.3 Agendamento de Tarefas
```bash
# Cron
crontab -e
/etc/crontab
/etc/cron.d/

# Systemd timers
systemctl list-timers
```

## Módulo 7 - Rede e Conectividade

### 7.1 Configuração de Rede
```bash
# Comandos de rede
ip addr, ip link, ip route
ifconfig (deprecated)
netstat, ss
ping, traceroute, tracepath
dig, nslookup, host
```

### 7.2 Network Manager
```bash
# Interface de linha de comando
nmcli connection show
nmcli device status
nmcli connection up/down
```

### 7.3 Firewall
```bash
# UFW (Uncomplicated Firewall)
sudo ufw enable
sudo ufw disable
sudo ufw status verbose
sudo ufw allow 22/tcp
sudo ufw deny from 192.168.1.100
```

### 7.4 Troubleshooting de Rede
```bash
# Diagnóstico
tcpdump
wireshark (GUI)
nmap
mtr
```

## Módulo 8 - Segurança Básica

### 8.1 Hardening do Sistema
```bash
# Atualizações de segurança
sudo unattended-upgrades

# Configuração de SSH
/etc/ssh/sshd_config
# Desativar root login
# Usar chaves SSH

# Auditoria
sudo lynis audit system
```

### 8.2 SELinux/AppArmor
```bash
# AppArmor (padrão no Ubuntu/Mint)
sudo aa-status
sudo aa-enforce /etc/apparmor.d/*
```

### 8.3 Logs e Monitoramento
```bash
# Arquivos de log importantes
/var/log/auth.log
/var/log/syslog
/var/log/kern.log
/var/log/apt/

# Ferramentas
logrotate configuração
fail2ban (proteção contra brute force)
```

## Módulo 9 - Scripts e Automação

### 9.1 Shell Scripting Básico
```bash
#!/bin/bash
# Estruturas básicas
# Variáveis
# Condicionais (if, case)
# Loops (for, while)
# Funções
# Parâmetros ($1, $2, $@, $#)
```

### 9.2 Scripts de Exemplo
```bash
#!/bin/bash
# Backup simples
BACKUP_DIR="/backup"
SOURCE_DIR="/home/usuario"
DATE=$(date +%Y%m%d)

tar -czf $BACKUP_DIR/backup_$DATE.tar.gz $SOURCE_DIR
echo "Backup concluído em $BACKUP_DIR/backup_$DATE.tar.gz"
```

### 9.3 Automação com Ansible (Introdução)
```yaml
# Exemplo playbook básico
- name: Instalar pacotes essenciais
  hosts: all
  become: yes
  tasks:
    - name: Instalar htop
      apt:
        name: htop
        state: present
```

## Módulo 10 - Monitoramento e Troubleshooting

### 10.1 Monitoramento de Sistema
```bash
# Comandos úteis
vmstat 1 10
iostat -xz 1
mpstat -P ALL 1
free -h
df -h
```

### 10.2 Logs e Diagnóstico
```bash
# Análise de logs
dmesg | tail -20
journalctl --since "1 hour ago"
journalctl -p err..alert

# Ferramentas gráficas
gnome-system-monitor
stacer (third-party)
```

### 10.3 Troubleshooting Metodologia
1. Identificar o problema
2. Coletar informações
3. Isolar a causa
4. Implementar solução
5. Verificar funcionamento
6. Documentar

## Módulo 11 - Serviços de Rede

### 11.1 Servidor Web (Apache/Nginx)
```bash
# Apache
sudo apt install apache2
sudo systemctl enable apache2

# Nginx
sudo apt install nginx
sudo systemctl enable nginx
```

### 11.2 Servidor de Banco de Dados
```bash
# MySQL/MariaDB
sudo apt install mariadb-server
sudo mysql_secure_installation

# PostgreSQL
sudo apt install postgresql postgresql-contrib
```

### 11.3 Servidor de Arquivos
- Samba (Windows compartilhamento)
- NFS (Network File System)
- FTP/VSFTPD

## Módulo 12 - Virtualização e Containers

### 12.1 VirtualBox/KVM
```bash
# KVM no Linux Mint
sudo apt install qemu-kvm libvirt-daemon-system \
libvirt-clients bridge-utils virt-manager
```

### 12.2 Docker
```bash
# Instalação
sudo apt install docker.io
sudo systemctl enable docker
sudo usermod -aG docker $USER

# Comandos básicos
docker ps
docker images
docker run hello-world
```

### 12.3 LXC/LXD
```bash
# Containers do sistema
sudo apt install lxd lxd-client
sudo lxd init
lxc launch ubuntu:22.04 meu-container
```

## Projetos Práticos

### Projeto 1: Servidor Web Completo
1. Instalar Linux Mint Server (ou desktop como servidor)
2. Configurar firewall
3. Instalar e configurar Nginx
4. Configurar virtual hosts
5. Implementar SSL com Let's Encrypt
6. Configurar monitoramento básico

### Projeto 2: Servidor de Backup
1. Configurar servidor de backup
2. Implementar rsync com scripts
3. Configurar backup incremental
4. Automatizar com cron
5. Testar restauração

### Projeto 3: Ambiente de Desenvolvimento
1. Configurar servidor Git
2. Implementar CI/CD básico
3. Configurar ambientes de staging/produção
4. Implementar monitoramento
5. Documentar processos

## Recursos Adicionais

### Livros Recomendados
1. "How Linux Works" - Brian Ward
2. "The Linux Command Line" - William Shotts
3. "Linux Administration Handbook" - Evi Nemeth

### Websites e Comunidades
- [Linux Mint Forums](https://forums.linuxmint.com/)
- [Ubuntu Documentation](https://help.ubuntu.com)
- [Arch Wiki](https://wiki.archlinux.org/) (avançado)
- [Stack Overflow](https://stackoverflow.com/)

### Cursos Online
- Linux Foundation cursos
- Red Hat training
- Udemy/Linux Academy

## Certificações Recomendadas

### Nível Iniciante
- Linux Essentials (LPI)

### Nível Intermediário
- LPIC-1
- CompTIA Linux+

### Nível Avançado
- LPIC-2/3
- RHCSA (Red Hat Certified System Administrator)

---

**Nota:** Este manual é um guia completo, mas a prática constante é essencial para dominar as habilidades de SysAdmin. Configure laboratórios virtuais, experimente, quebre coisas e aprenda a consertá-las. A comunidade Linux é uma grande aliada - não hesite em pedir ajuda quando necessário.

**Última atualização:** Outubro 2023
**Autor:** Manual SysAdmin Linux Mint
**Licença:** Creative Commons Attribution-ShareAlike
