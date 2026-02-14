
# 📚 Manual Completo com 100 Macetes do Windows
## Para Arquivo .MD (Markdown)

## 📑 ÍNDICE RÁPIDO
1. [Atalhos de Teclado Essenciais](#1-atalhos-de-teclado-essenciais)
2. [Personalização e Aparência](#2-personalização-e-aparência)
3. [Gerenciamento de Arquivos](#3-gerenciamento-de-arquivos)
4. [Segredos do Prompt e PowerShell](#4-segredos-do-prompt-e-powershell)
5. [Registro do Windows (Regedit)](#5-registro-do-windows-regedit)
6. [Internet e Rede](#6-internet-e-rede)
7. [Performance e Otimização](#7-performance-e-otimização)
8. [Segurança e Privacidade](#8-segurança-e-privacidade)
9. [Solução de Problemas](#9-solução-de-problemas)
10. [Produtividade Extrema](#10-produtividade-extrema)

---

## 1. ATALHOS DE TECLADO ESSENCIAIS

### #001 - Área de Trabalho Virtual
`Windows + Ctrl + D` = Cria nova área de trabalho virtual  
`Windows + Ctrl + ←/→` = Navega entre áreas  
`Windows + Ctrl + F4` = Fecha área atual

### #002 - Gerenciador de Tarefas Direto
`Ctrl + Shift + Esc` = Abre direto o Gerenciador (sem passar pela tela de segurança)

### #003 - Print Seletivo
`Windows + Shift + S` = Abre a ferramenta de recorte para capturar apenas uma parte da tela

### #004 - Bloquear PC Imediatamente
`Windows + L` = Bloqueia o PC na hora (útil para pausas rápidas)

### #005 - Menu Secreto Iniciar
`Windows + X` = Abre o menu de acesso rápido com ferramentas administrativas

### #006 - Minimizar Tudo Exceto Atual
`Windows + Home` = Minimiza todas as janelas, menos a que você está usando

### #007 - Espelhar Tela
`Windows + P` = Abre opções de projeção (duplicar, estender, etc.)

### #008 - Propriedades do Sistema Rápido
`Windows + Pause/Break` = Abre as propriedades do sistema diretamente

### #009 - Explorador em Modo Admin
`Ctrl + Shift + Enter` (com Explorador selecionado) = Abre como administrador

### #010 - Histórico de Área de Transferência
`Windows + V` = Abre o histórico de itens copiados (ativar nas configurações)

### #011 - Emojis e Caracteres Especiais
`Windows + .` (ponto final) ou `Windows + ;` = Abre o painel de emojis

### #012 - Fechar Janela por Aplicativo
`Ctrl + W` = Fecha a aba atual  
`Alt + F4` = Fecha o programa inteiro

### #013 - Navegação Rápida no Explorador
`Alt + ↑` = Sobe um nível na hierarquia de pastas  
`Alt + ←/→` = Volta/Avança no histórico

### #014 - Menu de Link Simbólico
`Shift + Botão Direito` em um arquivo = Aparece a opção "Copiar como caminho"

### #015 - Teclado Virtual de Emergência
`Windows + Ctrl + O` = Ativa o teclado virtual na tela

---

## 2. PERSONALIZAÇÃO E APARÊNCIA

### #016 - Modo Deus (God Mode)
Crie uma nova pasta e nomeie como:  
`GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}`  
Acesso a TODAS as configurações do Windows em um só lugar.

### #017 - Pasta Invisível
1. Crie uma pasta normal  
2. Botão direito > Propriedades > Personalizar > Alterar ícone  
3. Escolha um ícone em branco (espaços vazios)  
4. Renomeie segurando `Alt` + digite `0160` (teclado numérico)

### #018 - Barra de Tarefas Transparente
Use o registro:  
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`  
Crie DWORD `UseOLEDTaskbarTransparency` = 1

### #019 - Acelerar Menus
Registro: `HKEY_CURRENT_USER\Control Panel\Desktop`  
Altere `MenuShowDelay` de 400 para 50 (velocidade dos menus)

### #020 - Remover Setas de Atalhos
Registro: `HKEY_CLASSES_ROOT\lnkfile`  
Renomeie `IsShortcut` para `IsShortcut.old`  
Reinicie o explorer.exe

### #021 - Tema Windows Clássico (Modo Retrô)
`Windows + R` > `control.exe /name Microsoft.Personalization /page pageWallpaper`  
Escolha temas de alto contraste para visual antigo.

### #022 - Nome do Computador Personalizado
`Windows + R` > `sysdm.cpl` > Nome do Computador > Alterar

### #023 - Pastas Coloridas Sem Programas
Use ícones personalizados baixados da internet em formato .ico nas propriedades da pasta.

### #024 - Central de Ações Customizada
`Windows + R` > `gpedit.msc` (Pro) > Modelos Administrativos > Menu Iniciar > Remover notificações

### #025 - Habilitar Tema Escuro em Apps Antigos
`Windows + R` > `regedit`  
`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize`  
`AppsUseLightTheme` = 0

### #026 - Barra de Tarefas com Múltiplas Linhas
Desbloqueie a barra de tarefas e arraste a borda superior para cima para criar múltiplas linhas.

### #027 - Relógio com Segundos
`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`  
Crie DWORD `ShowSecondsInSystemClock` = 1

### #028 - Barras de Ferramentas Customizadas
Botão direito na barra de tarefas > Barras de ferramentas > Nova barra > Selecione uma pasta com atalhos.

### #029 - Central de Notificações Minimalista
Desative ícones da barra de tarefas em Configurações > Personalização > Barra de tarefas.

### #030 - Wallpaper Rotativo para Tela Bloqueada
Configurações > Personalização > Tela de bloqueio > Apresentação de slides.

---

## 3. GERENCIAMENTO DE ARQUIVOS

### #031 - Renomear Múltiplos Arquivos de Uma Vez
Selecione vários arquivos, pressione `F2`, digite um nome. Todos serão nomeados como "Nome (1)", "Nome (2)"...

### #032 - Converter .Docx para .Zip (Remover Senha)
1. Renomeie `.docx` para `.zip`  
2. Extraia o conteúdo  
3. Vá em `word/settings.xml`  
4. Delete a linha `<w:documentProtection...>`  
5. Recompacte e renomeie para .docx

### #033 - Abrir Arquivos Corrompidos (Word/Excel)
Importe o arquivo corrompido dentro do Word usando "Abrir e Reparar" (na seta ao lado do botão Abrir).

### #034 - Caminho Completo na Barra de Título
Opções de pasta > Modo de exibição > "Exibir caminho completo na barra de título".

### #035 - Link Simbólico (Pasta em Dois Lugares)
`mklink /J "C:\Destino" "D:\Origem"` (Prompt Admin)  
Cria uma pasta que aponta para outra unidade.

### #036 - Histórico de Arquivos (Versões Anteriores)
Botão direito no arquivo/pasta > Propriedades > Versões anteriores. Funciona se o backup estiver ativo.

### #037 - Copiar Lista de Arquivos para Txt
`dir /b > lista.txt` dentro da pasta desejada (Prompt).

### #038 - Arquivo .Bat para Desligar
Crie um arquivo .txt, renomeie para `desligar.bat` com o conteúdo:  
`shutdown /s /t 0`

### #039 - Atalho para Travar PC
Crie atalho com destino: `rundll32.exe user32.dll,LockWorkStation`

### #040 - Ocultar Arquivo Dentro de Imagem
`copy imagem.jpg + arquivo.rar imagem_falsa.jpg` (Prompt)  
Quem abrir verá a foto, mas pode extrair o .rar com WinRAR.

### #041 - Recuperar Arquivos Deletados da Lixeira (se vazia)
Use ferramentas como Recuva, mas rapidamente: pare de usar o disco imediatamente.

### #042 - Arquivo .BAT que se Auto-Deleta
`start /b "" cmd /c del "%~f0" & exit` no final do batch.

### #043 - Remover Metadados de Fotos
Botão direito na foto > Propriedades > Detalhes > Remover Propriedades.

### #044 - Desbloquear Arquivos Baixados da Internet
PowerShell: `Get-ChildItem -Path 'c:\pasta' -Recurse | Unblock-File`

### #045 - Forçar Exclusão de Arquivo em Uso
Use o LockHunter (programa) ou Prompt: `del /f arquivo`

---

## 4. SEGREDOS DO PROMPT E POWERSHELL

### #046 - Desligar em X Segundos
`shutdown /s /t 60` (desliga em 60 segundos)  
`shutdown /a` para abortar

### #047 - Ver Programas que Usam a Internet
`netstat -b -a` (mostra conexões e programas)

### #048 - Ver Chave do Produto Windows
`wmic path softwarelicensingservice get OA3xOriginalProductKey`

### #049 - Ping Contínuo com Timestamp
`ping -t google.com | cmd /q /v /c "(pause&pause)>nul & for /l %a in () do (set /p "data=" && echo(!date! !time! !data!)&ping -n 2 localhost >nul"`

### #050 - Reparar Arquivos do Sistema
`sfc /scannow` (verifica integridade)  
`DISM /Online /Cleanup-Image /RestoreHealth` (repara a imagem)

### #051 - Ver Bateria do Notebook (Relatório)
`powercfg /batteryreport` (gera HTML com detalhes)

### #052 - Esvaziar DNS Cache
`ipconfig /flushdns`

### #053 - Ver Wi-Fi Salvo e Senhas
`netsh wlan show profile name="NOMEDAREDE" key=clear`

### #054 - Comando para Abrir Programas como Admin
`runas /user:Administrador "caminho\programa.exe"`

### #055 - Criar Usuário Oculto
`net usuario nome$ senha /add` (o $ oculta da tela de login)

### #056 - Ver Histórico de Comandos
`doskey /history` (no CMD)

### #057 - Limpar Arquivos Temporários
`cleanmgr /sagerun:1` (executa limpeza automática)

### #058 - Iniciar em Modo de Segurança pelo CMD
`bcdedit /set {default} safeboot minimal`  
Para voltar: `bcdedit /deletevalue {default} safeboot`

### #059 - Ver Informações do Sistema
`systeminfo`

### #060 - Abrir Programas com Parâmetros
`start "" "caminho.exe" -parametro`

---

## 5. REGISTRO DO WINDOWS (REGEDIT)

### #061 - Adicionar "Abrir com Bloco de Notas" no Menu
`HKEY_CLASSES_ROOT\*\shell`  
Crie chave `Abrir com Bloco` > chave `command`  
Valor: `notepad.exe "%1"`

### #062 - Remover Seta de Atalho (versão alternativa)
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Icons`  
Crie string `29` com valor `%windir%\System32\imageres.dll,197`

### #063 - Desabilitar Tecla Windows
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout`  
Crie binário `Scancode Map` com valor: 00,00,00,00,00,00,00,00,03,00,00,00,00,00,5B,E0,00,00,5C,E0,00,00,00,00

### #064 - Remover Pastas do "Este Computador"
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions`  
Navegue e remova GUIDs indesejados.

### #065 - Velocidade do Teclado Aumentada
`HKEY_CURRENT_USER\Control Panel\Keyboard`  
`KeyboardDelay` = 0 | `KeyboardSpeed` = 48

### #066 - Desativar Windows Defender Permanentemente
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender`  
Crie DWORD `DisableAntiSpyware` = 1

### #067 - Adicionar "Executar como Administrator" Fixo
`HKEY_CLASSES_ROOT\exefile\shell\runas`

### #068 - Tirar Logon Automático (sem senha)
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon`  
`AutoAdminLogon` = 1  
`DefaultUserName` = seu nome  
`DefaultPassword` = sua senha

### #069 - Personalizar Lixeira (sem esvaziar)
`HKEY_CLASSES_ROOT\CLSID\{645FF040-5081-101B-9F08-00AA002F954E}\ShellFolder`  
Altere permissões para modificar o ícone.

### #070 - Remover Flecha do Atalho (Backup)
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Icons`  
Crie string `29` com valor `%windir%\System32\shell32.dll,50`

---

## 6. INTERNET E REDE

### #071 - Descobrir Senha do Wi-Fi Atual
`netsh wlan show profile name="SSID" key=clear` (olhar "Conteúdo da Chave")

### #072 - Liberar IP e Renovar
`ipconfig /release`  
`ipconfig /renew`

### #073 - Sites Bloqueados? Use DNS Alternativo
Painel de Controle > Central de Rede > Adaptador > IPv4 > DNS: 8.8.8.8 e 8.8.4.4 (Google)

### #074 - Ver Quem Está na Sua Rede
`arp -a` (mostra IPs e MACs conectados)

### #075 - Compartilhar Internet por Cabo (Bridge)
Selecione duas conexões no Painel de Rede e use "Conexão de ponte".

### #076 - Abrir Portas no Firewall
`netsh advfirewall firewall add rule name="Nome" dir=in action=allow protocol=TCP localport=8080`

### #077 - Mapear Unidade de Rede
`net use Z: \\servidor\pasta /persistent:yes`

### #078 - Testar Velocidade da Rede (Latência)
`ping -t 8.8.8.8`

### #079 - Descobrir Gateway Padrão
`ipconfig | findstr "Gateway"`

### #080 - Bloquear Sites via Hosts
Editar `C:\Windows\System32\drivers\etc\hosts` como admin  
Adicione: `127.0.0.1 www.site.com`

---

## 7. PERFORMANCE E OTIMIZAÇÃO

### #081 - Desativar Efeitos Visuais
`Windows + R` > `sysdm.cpl` > Avançado > Configurações de desempenho > Melhor desempenho

### #082 - Acelerar Desligamento (Forçar Fechamento)
Registro: `HKEY_CURRENT_USER\Control Panel\Desktop`  
`AutoEndTasks` = 1  
`HungAppTimeout` = 1000  
`WaitToKillAppTimeout` = 2000

### #083 - Desativar Programas na Inicialização
`Ctrl + Shift + Esc` > Inicializar > Desabilitar desnecessários

### #084 - Limpeza Profunda com Comando
`cleanmgr /sageset:1` (escolhe o que limpar)  
`cleanmgr /sagerun:1` (executa)

### #085 - Desfragmentar Disco (HD apenas, não SSD)
`dfrgui.exe`

### #086 - Verificar Saúde do HD
`wmic diskdrive get status` (no CMD)

### #087 - Desativar Indexação
Botão direito no disco > Propriedades > Desmarcar "Permitir indexação"

### #088 - Aumentar Memória Virtual (Swap)
Configurações > Sistema > Sobre > Configurações avançadas > Desempenho > Avançado > Memória virtual

### #089 - Modo de Jogo (Game Mode)
`Windows + G` > Configurações > Marcar "Usar modo de jogo"

### #090 - Priorizar Processos (Alta Prioridade)
`Ctrl + Shift + Esc` > Detalhes > Botão direito no processo > Definir prioridade

---

## 8. SEGURANÇA E PRIVACIDADE

### #091 - Esconder Usuário da Tela de Login
Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\SpecialAccounts\UserList`  
Crie DWORD com nome do usuário e valor 0

### #092 - Bloquear Acesso a Unidade
`gpedit.msc` > Configuração do Usuário > Modelos Administrativos > Componentes do Windows > Windows Explorer > Impedir acesso a unidades

### #093 - Histórico de Arquivos Abertos
`%appdata%\Microsoft\Windows\Recent` (pastas de atalhos recentes)

### #094 - Limpar Histórico de Executar
`Windows + R` > digite `regedit`  
Apague: `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`

### #095 - Criptografar Pasta (Sem Programas)
Botão direito na pasta > Propriedades > Avançados > Criptografar conteúdo (somente NTFS, faz backup da chave!)

### #096 - Desabilitar Webcam/Mic para Todos
`devmgmt.msc` > Desabilitar drivers de câmera/microfone

### #097 - Ver Log de Tentativas de Acesso
`eventvwr.msc` > Logs do Windows > Segurança

### #098 - Ativar Firewall para Rede Pública
Painel de Controle > Firewall > Ativar em todas as redes

### #099 - Desabilitar Telemetria (Coleta de Dados)
`gpedit.msc` > Configuração do Computador > Modelos Administrativos > Componentes do Windows > Coleta de Dados > Permitir Telemetria = 0

### #100 - Biometria Fake? Use PIN
Configurações > Contas > Opções de entrada > Adicionar PIN (mais rápido que senha)

---

## 🎁 BÔNUS: 5 MACETES EXTRAS DE EMERGÊNCIA

### #101 - Atalho para Travar o PC Imediatamente
Crie atalho: `%windir%\system32\rundll32.exe user32.dll, LockWorkStation`

### #102 - Desativar Tela ao Tocar no Mouse
Painel de Controle > Mouse > Hardware > Propriedades > Gerenciamento de energia > Desmarcar "Permitir que este dispositivo desperte o computador"

### #103 - Recuperar Arquivos Deletados com Sombra
Use o comando `vssadmin list shadows` e ferramentas como ShadowExplorer

### #104 - Abrir MSConfig Rápido
`Windows + R` > `msconfig`

### #105 - Ver Erros de Tela Azul (BSOD)
`%localappdata%\CrashDumps` ou ferramenta BlueScreenView

---

## ⚠️ AVISOS IMPORTANTES

1. **Registro do Windows:** Sempre faça backup do registro antes de modificar (Arquivo > Exportar).
2. **Prompt como Admin:** Muitos comandos só funcionam com privilégios de administrador.
3. **Backup:** Antes de usar macetes que alteram senhas ou sistema, tenha um backup ou ponto de restauração.
4. **Uso Ético:** Estas técnicas devem ser usadas apenas em equipamentos próprios ou com autorização.
5. **Atualizações:** Alguns macetes podem parar de funcionar com atualizações do Windows.

---
