## Índice
1.  [Interpretando os Resultados do Nmap](#interpretando-os-resultados-do-nmap)
2.  [Tabela das Principais Portas e Serviços](#tabela-das-principais-portas-e-serviços)
3.  [Como Acessar e Explorar Cada Porta/Serviço](#como-acessar-e-explorar-cada-portaserviço)

---

## Interpretando os Resultados do Nmap

Antes de explorar as portas, é crucial entender o que o Nmap informa sobre elas. A "tabela de portas interessantes" no resultado de uma varredura lista o número da porta, o protocolo, o nome do serviço e o **estado** da porta . Os estados possíveis são:

- **`open` (Aberta)**: Uma aplicação na máquina alvo está ativamente escutando e aceitando conexões/pacotes nessa porta . É o estado que mais nos interações para exploração ou auditoria.
- **`filtered` (Filtrada)**: Provavelmente existe um firewall, filtro de pacotes ou outra regra de rede bloqueando a porta. O Nmap não consegue determinar se a porta está aberta ou fechada .
- **`closed` (Fechada)**: A porta está acessível (responde ao Nmap), mas não há nenhuma aplicação escutando nela .
- **`unfiltered` (Não-filtrada)**: A porta responde às sondagens do Nmap, mas ele não consegue determinar se está aberta ou fechada .

## Tabela das Principais Portas e Serviços

A tabela abaixo lista as portas TCP mais comuns, seus serviços associados e uma breve descrição . Os comandos de acesso na próxima seção pressupõem que estas portas estejam no estado `open`.

| Porta | Serviço | Nome do Serviço | Descrição Resumida |
| :--- | :--- | :--- | :--- |
| **21** | `ftp` | File Transfer Protocol | Protocolo de transferência de arquivos. Pode permitir upload/download anônimo ou com autenticação. |
| **22** | `ssh` | Secure Shell | Acesso remoto seguro a linha de comando. Muito comum em servidores Linux/Unix. |
| **23** | `telnet` | Telnet | Acesso remoto a linha de comando (não criptografado). Tráfego visível em texto claro. |
| **25** | `smtp` | Simple Mail Transfer Protocol | Envio de e-mails. Pode ser explorado para envio de spam ou coleta de informações (VRFY, EXPN). |
| **53** | `domain` | Domain Name System (DNS) | Tradução de nomes de domínio para IP. Pode ser usado para ataques de cache poisoning ou coleta de zona. |
| **80** | `http` | Hypertext Transfer Protocol | Servidor web (páginas de internet). Ponto de entrada para ataques em aplicações web. |
| **110** | `pop3` | Post Office Protocol v3 | Recebimento de e-mails (cliente baixa os e-mails). Comunicação geralmente em texto claro. |
| **111** | `sunrpc` | RPC Portmapper (rpcbind) | Mapeamento de serviços RPC (NFS, NIS). Pode vazar informações sobre a rede. |
| **135** | `msrpc` | Microsoft RPC | Serviço RPC do Windows. Fundamental para administração do Windows. Altamente explorado no passado. |
| **139** | `netbios-ssn` | NetBIOS Session Service | Compartilhamento de arquivos e impressoras em redes Windows (legado). |
| **143** | `imap` | Internet Message Access Protocol | Recebimento de e-mails (cliente gerencia e-mails no servidor). Versão mais moderna que POP3. |
| **443** | `https` | HTTP over TLS/SSL | Servidor web com criptografia (SSL/TLS). Mesmas funções da porta 80, porém seguro. |
| **445** | `microsoft-ds` | SMB Direct | Compartilhamento de arquivos e impressoras do Windows (versão moderna, após o NetBIOS). Crítico para segurança (ex: EternalBlue). |
| **993** | `imaps` | IMAP over TLS/SSL | IMAP criptografado. |
| **995** | `pop3s` | POP3 over TLS/SSL | POP3 criptografado. |
| **1723** | `pptp` | Point-to-Point Tunneling Protocol | Protocolo para VPN (PPTP). Considerado inseguro atualmente. |
| **3306** | `mysql` | MySQL Database | Sistema de gerenciamento de banco de dados MySQL/MariaDB. |
| **3389** | `ms-wbt-server` | Microsoft Terminal Server (RDP) | Remote Desktop Protocol (RDP) do Windows. Acesso remoto à interface gráfica. |
| **5432** | `postgresql` | PostgreSQL Database | Sistema de gerenciamento de banco de dados PostgreSQL. |
| **5900** | `vnc` | Virtual Network Computing | Acesso remoto à interface gráfica (alternativa ao RDP). |
| **6379** | `redis` | Redis | Armazenamento de estrutura de dados em memória (banco de dados chave-valor). |
| **27017**| `mongod` | MongoDB Database | Banco de dados NoSQL orientado a documentos. |

## Como Acessar e Explorar Cada Porta/Serviço

Para acessar e tentar interagir com os serviços encontrados, você precisará de ferramentas específicas. Os comandos abaixo são um ponto de partida para exploração manual.

### ⚠️ **Aviso Legal Importante**
> **Apenas escaneie e acesse sistemas e redes que você possui ou tem permissão explícita por escrito para testar.** O uso não autorizado destas técnicas pode ser ilegal e resultar em severas penalidades . Os exemplos a seguir são para fins educacionais e devem ser praticados apenas em laboratórios próprios ou plataformas autorizadas (como `scanme.nmap.org`).

### Instruções de Acesso e Exploração

#### **Porta 21 (FTP)**
- **Acesso:** `ftp <IP_ALVO>`
- **O que tentar:**
    - Login anônimo: Usuário `anonymous` ou `ftp`, senha qualquer ou e-mail.
    - Comando `ls -la` para listar arquivos após o login.
    - Comando `get <arquivo>` para baixar um arquivo.
    - Verificar permissões de escrita com `put <arquivo_teste.txt>`.

#### **Porta 22 (SSH)**
- **Acesso:** `ssh usuário@<IP_ALVO>` ou `ssh -p 22 usuário@<IP_ALVO>`
- **O que tentar:**
    - Tentar login com credenciais fracas/padrão (ex: `root:root`, `admin:admin`).
    - Coletar a banner de versão para verificar vulnerabilidades conhecidas.
    - Se conseguir acesso, explorar o sistema.

#### **Porta 23 (Telnet)**
- **Acesso:** `telnet <IP_ALVO>`
- **O que tentar:**
    - Observar se algum banner de login é exibido.
    - Tentar credenciais padrão. Lembre-se: todo o tráfego, incluindo senhas, é enviado em texto puro e pode ser interceptado.

#### **Portas 80 (HTTP) e 443 (HTTPS)**
- **Acesso:** Abrir no navegador: `http://<IP_ALVO>` ou `https://<IP_ALVO>`
- **O que tentar:**
    - Inspecionar o código fonte da página.
    - Usar ferramentas como `dirb`, `gobuster` ou `ffuf` para descobrir diretórios e arquivos ocultos (`dirb http://<IP_ALVO>`).
    - Procurar por painéis de login, áreas administrativas ou versões de CMS (WordPress, Joomla) vulneráveis.

#### **Porta 445 (SMB)**
- **Acesso:** Utilizar ferramentas como `smbclient` ou `enum4linux`.
- **O que tentar:**
    - Listar compartilhamentos: `smbclient -L //<IP_ALVO> -N` (conexão anônima) ou `smbclient -L //<IP_ALVO> -U <usuário>`.
    - Conectar a um compartilhamento: `smbclient //<IP_ALVO>/<COMPARTILHAMENTO> -N`.
    - Usar o script `enum4linux` para extrair informações de usuários e políticas do domínio.

#### **Porta 3306 (MySQL)**
- **Acesso:** `mysql -h <IP_ALVO> -u <usuário> -p`
- **O que tentar:**
    - Conectar com usuários padrão e senha em branco: `mysql -h <IP_ALVO> -u root`.
    - Se conectar, listar bancos de dados com `SHOW DATABASES;`.
    - **Nota:** É raro encontrar MySQL diretamente exposto sem autenticação, mas pode acontecer em ambientes mal configurados.

#### **Porta 3389 (RDP)**
- **Acesso:** No Windows, tecla `Win + R`, digitar `mstsc` e conectar ao IP. No Linux, usar `xfreerdp` ou `rdesktop`.
- **O que tentar:**
    - `xfreerdp /v:<IP_ALVO>` (Linux).
    - Tentar login com credenciais fracas ou verificar se aceita conexão sem exigir autenticação de rede (NLA).
