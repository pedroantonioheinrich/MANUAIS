## Índice
1.  [O que é Google Dorking?](#o-que-é-google-dorking)
2.  [Aviso Legal e Ética](#aviso-legal-e-ética)
3.  [Por Que Usar Google Dorks?](#por-que-usar-google-dorks)
4.  [Como Funciona: Operadores de Busca Avançada](#como-funciona-operadores-de-busca-avançada)
    - [Operadores Booleanos e Símbolos](#operadores-booleanos-e-símbolos)
    - [Operadores de Busca Avançada (Os Comandos)](#operadores-de-busca-avançada-os-comandos)
5.  [Guia de Referência Rápida (Cheat Sheet)](#guia-de-referência-rápida-cheat-sheet)
6.  [Lista de 300 Dorks para Consulta](#lista-de-300-dorks-para-consulta)
    - [Categoria 1: Diretórios Listados (Index Of)](#categoria-1-diretórios-listados-index-of)
    - [Categoria 2: Arquivos de Configuração e Código Fonte](#categoria-2-arquivos-de-configuração-e-código-fonte)
    - [Categoria 3: Credenciais e Informações Sensíveis](#categoria-3-credenciais-e-informações-sensíveis)
    - [Categoria 4: Bancos de Dados e Backups](#categoria-4-bancos-de-dados-e-backups)
    - [Categoria 5: Logs e Mensagens de Erro](#categoria-5-logs-e-mensagens-de-erro)
    - [Categoria 6: Painéis Administrativos e Páginas de Login](#categoria-6-painéis-administrativos-e-páginas-de-login)
    - [Categoria 7: Câmeras e Dispositivos IoT](#categoria-7-câmeras-e-dispositivos-iot)
    - [Categoria 8: Servidores e Serviços Expostos (FTP, SSH)](#categoria-8-servidores-e-serviços-expostos-ftp-ssh)
    - [Categoria 9: Vulnerabilidades Web (XSS, SQLi, LFI)](#categoria-9-vulnerabilidades-web-xss-sqli-lfi)
    - [Categoria 10: Nuvem e Armazenamento (Cloud)](#categoria-10-nuvem-e-armazenamento-cloud)
    - [Categoria 11: Vazamentos em Sites de Código (Pastebin, GitHub)](#categoria-11-vazamentos-em-sites-de-código-pastebin-github)
    - [Categoria 12: Documentos e Informações Pessoais](#categoria-12-documentos-e-informações-pessoais)
    - [Categoria 13: Informações de Rede e Subdomínios](#categoria-13-informações-de-rede-e-subdomínios)
    - [Categoria 14: Diversos e Bypass](#categoria-14-diversos-e-bypass)
7.  [Como se Proteger do Google Dorking](#como-se-proteger-do-google-dorking)

---

## O que é Google Dorking?

Google Dorking, também conhecido como Google Hacking, é uma técnica de pesquisa avançada que utiliza operadores e comandos especiais para refinar os resultados no Google . O objetivo é encontrar informações que não são facilmente acessíveis através de buscas comuns, como arquivos confidenciais, diretórios expostos, painéis de login, credenciais vazadas e sistemas vulneráveis .

O termo "dork" (inglês para "idiota") refere-se, no contexto, a pessoas ou administradores que, sem querer, deixam informações sensíveis expostas online, tornando-as fáceis de serem encontradas .

## Aviso Legal e Ética

**IMPORTANTE:** Este manual é estritamente para fins **educacionais e de auditoria de segurança da informação** .

- **Use com Permissão:** Aplicar estas técnicas em sistemas, redes ou sites sem a devida autorização é ilegal e antiético. Utilize apenas em seus próprios projetos ou em ambientes controlados onde você tenha permissão explícita para realizar testes de penetração .
- **Consequências:** O acesso não autorizado a dados pode resultar em graves penalidades legais, incluindo multas e prisão.
- **Responsabilidade:** O autor e as fontes consultadas não se responsabilizam pelo uso indevido das informações aqui contidas.

## Por Que Usar Google Dorks?

Quando utilizada de forma ética, esta técnica é extremamente valiosa para:

- **Testes de Penetração (Pentesting):** Identificar falhas de segurança e exposição de dados em sistemas de clientes (com autorização) .
- **OSINT (Open Source Intelligence):** Coletar informações passivamente sobre um alvo específico (footprinting) .
- **Auditoria de Segurança:** Verificar se arquivos internos, backups ou diretórios estão acidentalmente públicos .
- **Profissionais de TI:** Localizar informações vazadas da própria empresa para removê-las .

## Como Funciona: Operadores de Busca Avançada

O coração do Google Dorking são os operadores. Eles instruem o Google a procurar de forma específica. Abaixo, a explicação detalhada de cada um.

### Operadores Booleanos e Símbolos

Antes dos comandos, é crucial entender a lógica booleana que pode ser aplicada :

| Operador | Descrição | Exemplo |
| :--- | :--- | :--- |
| **`AND`** ou **`+`** | Os resultados devem conter **todos** os termos. O Google já assume isso por padrão. | `segurança AND informática` |
| **`OR`** ou **`\|`** | Os resultados podem conter um termo **ou** outro. | `"sistema de login" OR "página de login"` |
| **`-`** (menos) | Exclui termos ou operadores dos resultados. | `jaguar -carro` (busca pelo animal, excluindo o automóvel) |
| **`"..."`** (aspas) | Busca pela **frase exata**, na ordem especificada . | `"confidencial e sigiloso"` |
| **`*`** (asterisco) | **Curinga.** Substitui uma palavra desconhecida em uma frase . | `"o * é a alma do negócio"` (pode achar "o cliente é a alma", "o marketing é a alma") |
| **`(...)`** (parênteses) | Agrupa termos e comandos para criar buscas complexas . | `inurl:(admin \| login)` |
| **`..`** (dois pontos) | Busca por um **intervalo numérico** . | `câmera $100..$300` |

### Operadores de Busca Avançada (Os Comandos)

Estes são os verdadeiros "dorks". Eles direcionam a busca para partes específicas de um site ou arquivo.

| Operador | Descrição | Exemplo | Por que usar?  |
| :--- | :--- | :--- | :--- |
| **`site:`** | Restringe a busca a um domínio ou site específico . | `site:gov.br "relatório de auditoria"` | Foca a pesquisa em um alvo, como um governo (gov) ou universidade (edu). |
| **`filetype:`** ou **`ext:`** | Busca por arquivos de um tipo específico (PDF, DOCX, XLS, etc.) . | `filetype:pdf "plano de negócios"` | Encontra documentos, relatórios e apresentações que não são páginas web comuns. |
| **`intitle:`** | Busca por um termo no **título** da página HTML . | `intitle:"index of"` | Clássico para encontrar listagens de diretórios. |
| **`allintitle:`** | Similar ao `intitle`, mas exige que **todas** as palavras estejam no título . | `allintitle: acesso restrito login` | |
| **`inurl:`** | Busca por um termo no **endereço URL** da página . | `inurl:admin.php` | Encontra páginas administrativas, painéis de controle e diretórios específicos. |
| **`allinurl:`** | Exige que todas as palavras estejam na URL . | `allinurl: etc passwd` | |
| **`intext:`** | Busca por um termo no **corpo do texto** da página . | `intext:"DB_PASSWORD"` | Ideal para achar strings de código ou configuração dentro do conteúdo da página. |
| **`allintext:`** | Exige que todas as palavras estejam no texto da página . | `allintext: username password filetype:log` | |
| **`cache:`** | Mostra a versão da página armazenada no cache do Google . | `cache:exemplo.com.br` | Útil para ver uma versão antiga de um site que pode ter sido alterada ou tirada do ar. |
| **`link:`** | Encontra páginas que fazem **link** para um determinado endereço . | `link:exemplo.com` | (Não é mais tão preciso, mas ajuda a encontrar menções a um site). |
| **`related:`** | Encontra sites **relacionados** ou similares a um determinado URL . | `related:nytimes.com` | Útil para encontrar concorrentes ou fontes de informação semelhantes. |
| **`info:`** | Apresenta informações sobre uma página específica . | `info:exemplo.com` | |
| **`define:`** | Fornece definições de um termo . | `define:pentest` | |
| **`before:`** / **`after:`** | Filtra resultados publicados antes ou depois de uma data . | `vulnerabilidade before:2023-01-01` | Para pesquisas temporais. |
| **`AROUND(n)`** | Encontra páginas onde duas palavras estão a uma distância de até **n** palavras uma da outra . | `segurança AROUND(3) informação` | |

## Guia de Referência Rápida (Cheat Sheet)

| Comando | Descrição | Exemplo |
| :--- | :--- | :--- |
| `site:` | Busca em domínio específico | `site:exemplo.com` |
| `filetype:` | Busca por tipo de arquivo | `filetype:pdf` |
| `intitle:` | Termo no título da página | `intitle:"index of"` |
| `inurl:` | Termo no endereço da página | `inurl:admin` |
| `intext:` | Termo no corpo da página | `intext:"senha"` |
| `cache:` | Versão em cache da página | `cache:google.com` |
| `"..."` | Busca por frase exata | `"confidencial"` |
| `-` | Exclui termo | `-site:youtube.com` |
| `OR` (ou `\|`) | Um termo ou outro | `php OR asp` |
| `*` | Curinga (qualquer palavra) | `como * um computador` |
| `..` | Intervalo numérico | `$500..$1000` |
| `AROUND(n)` | Palavras próximas | `google AROUND(5) hack` |

## Lista de 300 Dorks para Consulta

Abaixo está uma compilação abrangente de 300 dorks, organizados por categoria para facilitar a consulta. **Lembre-se:** substitua "exemplo.com" ou "alvo" pelo domínio que você está auditando (com permissão).

### Categoria 1: Diretórios Listados (Index Of)
> Encontra listagens de diretórios abertos, que podem expor todos os arquivos de uma pasta.

| # | Dork |
| :--- | :--- |
| 1 | `intitle:"index of"`  |
| 2 | `intitle:"index of" inurl:admin` |
| 3 | `intitle:"index of" inurl:backup`  |
| 4 | `intitle:"index of" inurl:ftp` |
| 5 | `intitle:"index of" etc` |
| 6 | `intitle:"index of" config` |
| 7 | `intitle:"index of" .git/` |
| 8 | `intitle:"index of" .svn/` |
| 9 | `intitle:"index of" id_rsa`  |
| 10 | `intitle:"index of" "parent directory"` |
| 11 | `intitle:"index of" "docker-compose.yml"`  |
| 12 | `intitle:"index of" "credentials.json"`  |
| 13 | `intitle:"index of" "server-status"` |
| 14 | `intitle:"index of" "logs"` |
| 15 | `intitle:"index of" "database"` |
| 16 | `intitle:"index of" "sql"` |
| 17 | `intitle:"index of" "phpmyadmin"` |
| 18 | `intitle:"index of" "uploads"` |
| 19 | `intext:"index of /"` |
| 20 | `intext:"Index of /" +.htaccess` |

### Categoria 2: Arquivos de Configuração e Código Fonte
> Localiza arquivos que contêm configurações de sistemas, servidores e aplicações.

| # | Dork |
| :--- | :--- |
| 21 | `filetype:env "DB_PASSWORD"`  |
| 22 | `filetype:config intext:"password"` |
| 23 | `filetype:conf inurl:"server.conf"`  |
| 24 | `filetype:ini intext:"password"`  |
| 25 | `filetype:xml intext:"password"` |
| 26 | `filetype:json intext:"api_key"`  |
| 27 | `ext:yaml intext:"database_password"` |
| 28 | `ext:yml intext:"docker-compose"`  |
| 29 | `ext:sql intext:"INSERT INTO users"` |
| 30 | `filetype:php inurl:config.php` |
| 31 | `inurl:wp-config.php`  |
| 32 | `inurl:config.php.bak` |
| 33 | `inurl:configuration.php` (Joomla) |
| 34 | `filetype:htaccess htpasswd` |
| 35 | `ext:htpasswd` |
| 36 | `filetype:env intext:"AWS_SECRET_ACCESS_KEY"` |
| 37 | `filetype:json intext:"AWSAccessKeyId"`  |
| 38 | `filetype:json intext:"client_secret"`  |
| 39 | `ext:xml intext:"AWSAccessKeyId"`  |
| 40 | `intext:"DB_HOST" filetype:env` |
| 41 | `filetype:properties "database.password"` (Java) |
| 42 | `filetype:conf "server.xml"` (Tomcat) |
| 43 | `filetype:cnf "my.ini"` (MySQL) |
| 44 | `filetype:log "phpinfo()"` |
| 45 | `inurl:/phpinfo.php`  |
| 46 | `filetype:py intext:"password = "` |
| 47 | `filetype:js intext:"apikey"` |
| 48 | `ext:ps1 intext:"password"` (PowerShell) |
| 49 | `ext:sh intext:"password"` (Shell Script) |
| 50 | `intext:"<?php" intext:"mysql_connect"` |

### Categoria 3: Credenciais e Informações Sensíveis
> Busca por strings que parecem ser nomes de usuário, senhas ou tokens em texto puro.

| # | Dork |
| :--- | :--- |
| 51 | `intext:"username" intext:"password" filetype:txt`  |
| 52 | `intext:"password =" intext:"username =" filetype:log` |
| 53 | `intext:"@gmail.com" intext:"password"` |
| 54 | `intext:"-----BEGIN RSA PRIVATE KEY-----"` |
| 55 | `intext:"-----BEGIN DSA PRIVATE KEY-----"` |
| 56 | `intext:"-----BEGIN OPENSSH PRIVATE KEY-----"` |
| 57 | `filetype:pwd` |
| 58 | `filetype:key` |
| 59 | `intext:"passcode" intext:"login"` |
| 60 | `intext:"AWS_ACCESS_KEY_ID"` |
| 61 | `intext:"token" filetype:log` |
| 62 | `intext:"secret" inurl:credentials.json` |
| 63 | `intext:"root" intext:"password" ext:txt` |
| 64 | `intext:"user:pass" filetype:txt` |
| 65 | `intext:"senha" ext:txt` |
| 66 | `filetype:sql "VALUES ('admin', 'password'"` |
| 67 | `filetype:xls intext:username intext:password`  |
| 68 | `filetype:xlsx "email" "senha"` |
| 69 | `filetype:pdf "senha do sistema"` |
| 70 | `intext:"ftp://username:password@"` |

### Categoria 4: Bancos de Dados e Backups
> Localiza arquivos de banco de dados e backups completos que podem ter sido expostos.

| # | Dork |
| :--- | :--- |
| 71 | `filetype:sql intext:"-- MySQL dump"`  |
| 72 | `filetype:sql intext:"INSERT INTO" -` |
| 73 | `filetype:sql intext:"password"`  |
| 74 | `filetype:sql intext:"usuario"` |
| 75 | `filetype:bak inurl:"backup"` |
| 76 | `filetype:bak intext:"database"` |
| 77 | `ext:backup` |
| 78 | `ext:old`  |
| 79 | `ext:swp` (Arquivos de swap do Vim) |
| 80 | `intitle:"index of" "backup.zip"` |
| 81 | `intitle:"index of" "db.sql"` |
| 82 | `intitle:"index of" "dump.sql"` |
| 83 | `intitle:"index of" "backup.tar.gz"` |
| 84 | `inurl:backup filetype:sql` |
| 85 | `inurl:backup filetype:zip`  |
| 86 | `inurl:backup filetype:tar` |
| 87 | `filetype:sql "phpMyAdmin SQL Dump"` |
| 88 | `filetype:sql "INSERT INTO `admin`"` |
| 89 | `ext:dmp` (Oracle Dump) |
| 90 | `inurl:/.git/`  |

### Categoria 5: Logs e Mensagens de Erro
> Arquivos de log e páginas de erro podem revelar caminhos de sistema, versões de software e vulnerabilidades.

| # | Dork |
| :--- | :--- |
| 91 | `filetype:log intext:"password"`  |
| 92 | `filetype:log "error" intext:"Fatal"` |
| 93 | `filetype:log "access denied"` |
| 94 | `filetype:log intext:"root"` |
| 95 | `filetype:log "Apache" "error"` |
| 96 | `intext:"mysql_error()"` |
| 97 | `intext:"SQL syntax error"` |
| 98 | `intext:"Warning: mysql_connect()"` |
| 99 | `intext:"Parse error: syntax error" inurl:php` |
| 100 | `intext:"Warning: include("` |
| 101 | `intext:"Internal Server Error"`  |
| 102 | `intext:"StackTrace" intext:"Exception"` |
| 103 | `intitle:"Apache Status" inurl:/server-status`  |
| 104 | `intitle:"Apache2 Debian Default Page"` |
| 105 | `intitle:"Welcome to nginx!"`  |
| 106 | `inurl:/logs/ intitle:"index of"` |
| 107 | `intext:"Fatal error: Call to undefined function"` |
| 108 | `filetype:log "POST /wp-login.php"` |
| 109 | `intext:"[error] [client" filetype:log` |
| 110 | `filetype:txt "System Log"` |

### Categoria 6: Painéis Administrativos e Páginas de Login
> Portais de entrada para sistemas administrativos, muitas vezes sem proteção adequada.

| # | Dork |
| :--- | :--- |
| 111 | `inurl:admin`  |
| 112 | `inurl:login`  |
| 113 | `inurl:admin/login.php` |
| 114 | `inurl:administrator` (Joomla) |
| 115 | `inurl:wp-admin`  |
| 116 | `inurl:user/login` (Drupal) |
| 117 | `intitle:"Admin" inurl:admin` |
| 118 | `intitle:"Login Page" intext:"admin"` |
| 119 | `intitle:"Control Panel"` |
| 120 | `inurl:phpMyAdmin/index.php`  |
| 121 | `intitle:"phpMyAdmin" "running on"`  |
| 122 | `inurl:webadmin` |
| 123 | `inurl:cpanel` |
| 124 | `inurl:dashboard` |
| 125 | `inurl:portal` |
| 126 | `intitle:"Jenkins" inurl:/jenkins`  |
| 127 | `intitle:"Grafana" inurl:/login`  |
| 128 | `intitle:"Kibana" inurl:/app/kibana` |
| 129 | `intitle:"SonarQube" inurl:/dashboard`  |
| 130 | `intitle:"RouterOS" inurl:winbox`  |
| 131 | `intitle:"MikroTik RouterOS"` |
| 132 | `inurl:/owa/` (Outlook Web Access) |
| 133 | `inurl:/webmail/` |
| 134 | `inurl:/cgi-bin/` |
| 135 | `inurl:8080 intitle:"Dashboard"` |

### Categoria 7: Câmeras e Dispositivos IoT
> Encontra câmeras de segurança, webcams e outros dispositivos conectados à internet.

| # | Dork |
| :--- | :--- |
| 136 | `intitle:"webcamXP 5" inurl:8080`  |
| 137 | `intitle:"webcamXP" | inurl:"lvappl.htm"`  |
| 138 | `intitle:"Live View / - AXIS"` |
| 139 | `intitle:"Live View" inurl:LvAppl`  |
| 140 | `intitle:"WJ-NT104 Main Page"` (Panasonic) |
| 141 | `inurl:view/view.shtml` |
| 142 | `inurl:control/camerainfo` |
| 143 | `inurl:"CgiStart?page="` |
| 144 | `intitle:"Snap Server" inurl:home` |
| 145 | `intitle:"Toshiba Network Camera"` |
| 146 | `intitle:"Network Camera" inurl:main.cgi` |
| 147 | `inurl:top.htm inurl:currenttime` |
| 148 | `intitle:"H.264 IP CAMERA"` |
| 149 | `intitle:"DVR Client"` |
| 150 | `intitle:"webcam 7"` |

### Categoria 8: Servidores e Serviços Expostos (FTP, SSH)
> Encontra servidores FTP públicos, banners de SSH e outros serviços.

| # | Dork |
| :--- | :--- |
| 151 | `intitle:"FTP root at"` |
| 152 | `intitle:"index of" inurl:ftp` |
| 153 | `inurl:FTP "ftp root at"`  |
| 154 | `"Parent Directory" "Last modified" ftp`  |
| 155 | `intitle:"SSH" "Server banner"`  |
| 156 | `filetype:url +inurl:"ftp://"` |
| 157 | `inurl:/ftp/ intitle:"index of"`  |
| 158 | `intitle:"Directory Listing For" -html` |
| 159 | `intitle:"Vsftpd"` |
| 160 | `intitle:"ProFTPD Server"` |

### Categoria 9: Vulnerabilidades Web (XSS, SQLi, LFI)
> Identifica parâmetros em URLs que são conhecidos por serem vetores de ataque comuns.

| # | Dork |
| :--- | :--- |
| 161 | `inurl:q= | inurl:s= | inurl:search=`  |
| 162 | `inurl:id= | inurl:pid= | inurl:cat=`  |
| 163 | `inurl:file= | inurl:include | inurl:doc=`  |
| 164 | `inurl:redirect= | inurl:url= | inurl:return=`  |
| 165 | `inurl:cmd | inurl:exec= | inurl:ping=`  |
| 166 | `inurl:"search.php?q="`  |
| 167 | `inurl:".php?id="`  |
| 168 | `inurl:"/view.php?page="`  |
| 169 | `inurl:"redirect.php?url="`  |
| 170 | `inurl:"/etc/passwd" intext:root:x` (LFI) |
| 171 | `inurl:index.php?page= intext:include` |
| 172 | `inurl:"?page=home" & intext:"include"` |
| 173 | `inurl:forum?module= & intext:"Warning"` |
| 174 | `intitle:"Cross Site Request Forgery"`  |
| 175 | `intitle:"Open Redirect" inurl:url=` |
| 176 | `site:openbugbounty.org inurl:reports intext:"exemplo.com"`  |

### Categoria 10: Nuvem e Armazenamento (Cloud)
> Buckets da AWS, Azure, Google Cloud e outros serviços de armazenamento que podem estar públicos.

| # | Dork |
| :--- | :--- |
| 177 | `site:s3.amazonaws.com "exemplo.com"`  |
| 178 | `site:s3.amazonaws.com "index of"`  |
| 179 | `site:blob.core.windows.net "exemplo.com"`  |
| 180 | `site:googleapis.com "exemplo.com"`  |
| 181 | `site:drive.google.com inurl:"/d/" "exemplo.com"`  |
| 182 | `site:docs.google.com inurl:"/d/" "confidencial"` |
| 183 | `site:dropbox.com/s "exemplo.com"`  |
| 184 | `site:box.com/s "exemplo.com"`  |
| 185 | `site:dev.azure.com "exemplo.com"`  |
| 186 | `site:sharepoint.com "exemplo.com"`  |
| 187 | `site:digitaloceanspaces.com "exemplo.com"`  |
| 188 | `site:firebaseio.com "exemplo.com"`  |
| 189 | `site:jfrog.io "exemplo.com"`  |
| 190 | `intext:exemplo.com AND (site:s3.amazonaws.com \| site:blob.core.windows.net)` |
| 191 | `site:amazonaws.com intext:"AWS Secret"` |

### Categoria 11: Vazamentos em Sites de Código (Pastebin, GitHub)
> Serviços onde desenvolvedores e usuários podem, acidentalmente, postar informações sensíveis.

| # | Dork |
| :--- | :--- |
| 192 | `site:pastebin.com "exemplo.com"`  |
| 193 | `site:pastebin.com intext:"password"`  |
| 194 | `site:github.com "exemplo.com"` |
| 195 | `site:github.com "api_key" "exemplo.com"`  |
| 196 | `site:github.com intext:"AWS_ACCESS_KEY_ID"` |
| 197 | `site:gitlab.com "exemplo.com"` |
| 198 | `site:jsfiddle.net "exemplo.com"`  |
| 199 | `site:codebeautify.org "exemplo.com"`  |
| 200 | `site:codepen.io "exemplo.com"`  |
| 201 | `site:repl.it "exemplo.com"`  |
| 202 | `site:bitbucket.org "exemplo.com"`  |
| 203 | `site:trello.com "exemplo.com"`  |
| 204 | `site:atlassian.com "exemplo.com"`  |
| 205 | `site:npmjs.org "exemplo.com"` |

### Categoria 12: Documentos e Informações Pessoais
> Arquivos de documentos que podem conter dados pessoais, financeiros ou estratégicos.

| # | Dork |
| :--- | :--- |
| 206 | `filetype:pdf OR filetype:doc OR filetype:xlsx`  |
| 207 | `intitle:"curriculo" filetype:pdf` |
| 208 | `filetype:xls "cartão de crédito"` |
| 209 | `filetype:xls intext:"CPF"` |
| 210 | `filetype:pdf intext:"número do cartão"` |
| 211 | `filetype:pdf intext:"relatório anual"` |
| 212 | `intext:"número do RG" filetype:pdf` |
| 213 | `filetype:doc intext:"endereço" telefone` |
| 214 | `filetype:csv "nome" "email"` |
| 215 | `filetype:csv "Número do Seguro Social"` |
| 216 | `intext:"orçamento" filetype:xlsx` |
| 217 | `intext:"plano de negócios" ext:doc` |
| 218 | `intitle:"lista de clientes" filetype:xls` |
| 219 | `filetype:pdf "dados bancários"` |
| 220 | `filetype:txt "números de telefone"` |

### Categoria 13: Informações de Rede e Subdomínios
> Mapeia a estrutura de um domínio alvo.

| # | Dork |
| :--- | :--- |
| 221 | `site:*.exemplo.com`  |
| 222 | `site:*.*.exemplo.com`  |
| 223 | `site:exemplo.com -www`  |
| 224 | `site:exemplo.com inurl:192.168.*` (Endereços IP internos vazados) |
| 225 | `site:exemplo.com intranet` |
| 226 | `site:exemplo.com portal` |
| 227 | `inurl:8080 intitle:"login" site:exemplo.com` |
| 228 | `site:exemplo.com intext:"10.0.0."` (Rede interna Classe A) |
| 229 | `site:exemplo.com ext:xml inurl:.*` |

### Categoria 14: Diversos e Bypass
> Combinações variadas e técnicas para contornar buscas comuns.

| # | Dork |
| :--- | :--- |
| 230 | `cache:exemplo.com`  |
| 231 | `related:exemplo.com`  |
| 232 | `info:exemplo.com`  |
| 233 | `"powered by vBulletin" inurl:forum` |
| 234 | `"powered by WordPress" inurl:wp-content` |
| 235 | `"powered by Drupal"` |
| 236 | `intitle:"welcome to" intext:"please create"` (Instaladores de software) |
| 237 | `intitle:"installation wizard" intext:"next"` |
| 238 | `inurl:/proc/self/cwd`  |
| 239 | `inurl:email.xls ext:xls`  |
| 240 | `inurl:"/upload/" filetype:php`  |
| 241 | `inurl:download.php?file=` |
| 242 | `inurl:include.php?file=` |
| 243 | `intitle:"VNC viewer for Java"` |
| 244 | `intitle:"TightVNC" inurl:5800` |
| 245 | `inurl:8080 intitle:"Remote Desktop"` |
| 246 | `inurl:8443 intitle:"Remote Desktop"` |
| 247 | `intitle:"Remote Desktop Web Connection"` |
| 248 | `inurl:8080 intitle:"index of"` |
| 249 | `intitle:"RouterOS router configuration page"` |
| 250 | `inurl:443 intext:"login" intitle:"Router"` |
| 251 | `inurl:9000 intitle:"phpMyAdmin"` (Porta alternativa) |
| 252 | `intitle:"DokuWiki" inurl:start` |
| 253 | `intitle:"MediaWiki" inurl:index.php` |
| 254 | `inurl:/confluence/` |
| 255 | `inurl:/jira/` |
| 256 | `intitle:"GitLab" inurl:/users/sign_in` |
| 257 | `intitle:"portainer" intext:"Login"`  |
| 258 | `inurl:/smb/ intitle:"index of"`  |
| 259 | `intitle:"Kibana" intext:"Loading Kibana"` |
| 260 | `inurl:"/etc/" filetype:conf` |
| 261 | `intitle:"PHP Error" "mysql_error"` |
| 262 | `intext:"Call Stack" intext:"Error" filetype:log` |
| 263 | `intitle:"phpinfo()" intext:"PHP Version"` |
| 264 | `filetype:log "GET /" "POST /"` |
| 265 | `filetype:log "sshd" "Accepted password"` |
| 266 | `filetype:log "wp-admin" "POST"` |
| 267 | `intext:"USER" intext:"PASS" filetype:log` |
| 268 | `filetype:reg reg +intext:"defaultpassword"` |
| 269 | `filetype:inf intext:"password"` |
| 270 | `filetype:rdp inurl:default` (Remote Desktop) |
| 271 | `filetype:pdf "confidencial" site:gov.br` |
| 272 | `filetype:xls "licitação" site:gov.br` |
| 273 | `filetype:pdf "resultados de exames"` |
| 274 | `inurl:disciplina filetype:pdf` (Conteúdo educacional restrito) |
| 275 | `intitle:"Lista de Alunos" filetype:xls` |
| 276 | `intext:"@aluno.universidade.edu.br" filetype:txt` |
| 277 | `site:drive.google.com "aluno" "matrícula"` |
| 278 | `site:docs.google.com "orçamento"` |
| 279 | `inurl:/_layouts/ intitle:"Documents"` (SharePoint) |
| 280 | `site:amazonaws.com "Docker Registry"` |
| 281 | `intitle:"Hadoop" inurl:50070` (Hadoop Admin) |
| 282 | `intitle:"MongoDB" intext:"MongoDB Version"` |
| 283 | `inurl:5984/_utils/` (CouchDB Admin) |
| 284 | `intitle:"elasticsearch" inurl:9200` |
| 285 | `site:github.com "PERCONA" "password"` |
| 286 | `site:github.com "STRIPE_KEY"` |
| 287 | `site:github.com "slack_token"` |
| 288 | `site:github.com "PRIVATE_KEY" "BEGIN"` |
| 289 | `site:pastebin.com "PRIVATE KEY"` |
| 290 | `site:codepad.co "api_key"` |
| 291 | `site:scastie.scala-lang.org "password"` |
| 292 | `site:ideone.com "secret"` |
| 293 | `site:gist.github.com "access_token"` |
| 294 | `inurl:controlpanel intitle:"Login"` |
| 295 | `intitle:"Mail Administrator" inurl:roundcube` |
| 296 | `inurl:squirrelmail/src/login.php` |
| 297 | `inurl:/horde/imp/` |
| 298 | `intitle:"Webmail" inurl:/src/login.php` |
| 299 | `intitle:"Welcome to Kerio Connect"` |
| 300 | `filetype:ics ics` (Arquivos de calendário) |

## Como se Proteger do Google Dorking

A melhor defesa é a prevenção. Google Dorking explora **indexação inadequada**. Para proteger seus sistemas e dados:

1.  **Robots.txt:** Use o arquivo `robots.txt` para instruir o Google a não indexar diretórios e arquivos sensíveis (ex: `/admin`, `/backup`, `*.env`).
2.  **Autenticação:** Proteja diretórios administrativos e de backup com senhas fortes e, idealmente, autenticação de dois fatores. Não os deixe acessíveis publicamente.
3.  **Remoção de Arquivos:** Nunca armazene senhas, chaves de API ou informações confidenciais em texto puro em servidores web públicos.
4.  **Monitoramento (Egosurfing):** Pesquise regularmente por si mesmo ou pela sua empresa usando dorks para saber se algo vazou . Por exemplo: `site:seu-site.com.br filetype:pdf`.
5.  **Solicitação de Remoção:** Se encontrar conteúdo sensível indexado, entre em contato com o administrador do site e, se necessário, utilize as ferramentas de remoção de conteúdo do Google .
6.  **Scanner de Vulnerabilidades:** Utilize ferramentas automatizadas (vulnerability scanners) para verificar periodicamente se há exposição de dados .

Lembre-se: O conhecimento é uma ferramenta poderosa. Use este guia para construir sistemas mais seguros, não para invadi-los.
