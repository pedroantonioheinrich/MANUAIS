## Manual Completo de Uso do MikroTik RouterOS

### 1. Introdução ao Mundo MikroTik

**MikroTik** é uma empresa letã conhecida por seus roteadores e softwares de rede de alto desempenho e custo-benefício. O coração de seus dispositivos é o **RouterOS**, um sistema operacional baseado em Linux (kernel 3.3.5) que transforma um computador comum ou um hardware dedicado (RouterBOARD) em um roteador profissional, firewall, ponto de acesso wireless, e muito mais .

**Por que o MikroTik é tão popular?**
- **Versatilidade:** Pode ser usado em redes domésticas, pequenos escritórios, provedores de internet (ISPs) e grandes corporações.
- **Custo-Benefício:** Oferece funcionalidades presentes em equipamentos muito mais caros.
- **Flexibilidade:** Permite um nível de personalização quase infinito para atender às necessidades mais específicas .

### 2. Formas de Acesso e Ferramentas de Gerenciamento

Existem várias maneiras de gerenciar um roteador MikroTik. As mais comuns para iniciantes são:

- **Winbox:** A ferramenta gráfica (GUI) mais popular e recomendada para iniciantes. É um pequeno executável que se conecta ao roteador via IP ou MAC address, oferecendo uma interface intuitiva .
- **WebFig:** É a interface gráfica baseada em navegador (web). Útil quando você não pode ou não quer instalar o Winbox .
- **CLI (Command Line Interface):** Acesso via terminal (SSH, Telnet ou diretamente na tela do roteador). Oferece controle total e é essencial para automação e diagnósticos avançados .

### 3. Primeiros Passos: Conectando e Configurando o Roteador

#### 3.1. O que você precisa
- Um roteador MikroTik (RouterBOARD).
- Um computador com porta Ethernet.
- Um cabo de rede (LAN).
- Acesso à internet do seu provedor (ISP) para a configuração final .

#### 3.2. Conexão Física Inicial
1.  Conecte o cabo de rede do seu computador a qualquer porta do MikroTik, **exceto a primeira porta (geralmente chamada de ether1)**.
2.  Conecte o cabo de internet (do modem/ISP) à porta **ether1**.
3.  Ligue o roteador na energia e aguarde um minuto para a inicialização completa .

#### 3.3. Acessando com o Winbox
1.  **Baixe o Winbox** no site oficial da MikroTik (mikrotik.com/download). Nenhuma instalação é necessária; basta executar o arquivo.
2.  Abra o Winbox. Ele automaticamente procurará por roteadores MikroTik na rede local (na aba **Neighbors**).
3.  Você verá o seu roteador listado, geralmente com um endereço MAC. **Clique duas vezes na linha** correspondente.
4.  Uma janela de login aparecerá com o usuário **admin** e a senha em branco. Clique em **Connect** .

    > **Nota Importante:** Se for a primeira vez que você acessa, pode aparecer uma mensagem sobre a configuração padrão. Para iniciantes, é **altamente recomendado manter a configuração padrão**, pois ela já inclui regras básicas de segurança e um servidor DHCP .

### 4. Configuração Passo a Passo da Internet

Agora que você está dentro do Winbox, vamos configurar o roteador para ter acesso à internet.

#### 4.1. Identificando o Tipo de Conexão com o ISP
Primeiro, você precisa saber qual o método de conexão utilizado pelo seu provedor de internet:
- **DHCP (Dinâmico):** O mais comum para conexões residenciais via cabo. O roteador recebe todas as configurações automaticamente.
- **PPPoE:** Comum em conexões DSL (antigas) e algumas fibras ópticas. Você tem um **usuário e senha** fornecidos pelo ISP.
- **IP Estático:** Geralmente para conexões empresariais. O ISP fornece um IP, máscara, gateway e DNS fixos .

#### 4.2. Configurando a Interface WAN (Internet)
Vamos configurar a porta `ether1` como nossa porta de internet (WAN).

- **Se for DHCP:**
    1.  No Winbox, vá ao menu **IP > DHCP Client**.
    2.  Clique no botão **+** (adicionar).
    3.  Em **Interface**, selecione `ether1`.
    4.  Mantenha as outras opções como estão e clique em **OK**.
    5.  Em poucos segundos, o *Status* deve mudar para `bound` e um endereço IP aparecerá, indicando que o roteador já pegou as configurações do modem .

- **Se for PPPoE:**
    1.  Vá ao menu **PPP**.
    2.  Na aba **Interfaces**, clique no botão **+** e escolha **PPPoE Client**.
    3.  Na aba **General**, defina um nome (ex: `pppoe-out1`) e escolha `ether1` como interface.
    4.  Vá para a aba **Dial Out**. Preencha o **User** (usuário) e **Password** fornecidos pelo seu ISP.
    5.  Marque as opções **Use Peer DNS** e **Add Default Route**.
    6.  Clique em **OK**. A interface ficará com a flag "R" (running) se a conexão for bem-sucedida .

#### 4.3. Verificando a Conectividade
Para ter certeza de que o roteador está conectado à internet, vamos usar a ferramenta de `ping`.
1.  No Winbox, vá ao menu **Tools > Ping**.
2.  No campo **Ping To**, digite um endereço como `8.8.8.8` (DNS do Google).
3.  Clique em **Start**. Se você vir as respostas ("Reply from..."), a conexão do roteador com a internet está funcionando .

### 5. Configurando a Rede Local (LAN)

Para que os computadores e dispositivos da sua rede local possam se conectar e ganhar IP automaticamente, você precisa configurar um servidor DHCP na interface LAN (por exemplo, `ether2`, `ether3`, etc.).

#### 5.1. Configurando o Servidor DHCP
O Winbox tem um assistente que facilita muito esse processo.
1.  Vá ao menu **IP > DHCP Server**.
2.  Clique no botão **DHCP Setup** (assistente).
3.  Na janela que abrir, selecione a interface de rede que será usada para a LAN (por exemplo, `bridge1` ou `ether2`). Clique em **Next**.
4.  O assistente irá sugerir um espaço de rede (`192.168.88.0/24`), um gateway (`192.168.88.1`) e um pool de endereços para distribuir (`192.168.88.2` a `192.168.88.254`). Você pode aceitar as sugestões ou personalizar. Continue clicando em **Next** até finalizar .

Agora, qualquer dispositivo que você conectar às portas LAN do MikroTik receberá um IP automaticamente e estará na rede local.

### 6. Liberando o Acesso à Internet: O NAT

Com o roteador conectado à internet e a LAN configurada, falta um passo crucial: o **NAT (Network Address Translation)**. O NAT permite que todos os dispositivos da sua rede local (com IPs privados) compartilhem uma única conexão de internet (com IP público) .

1.  Vá ao menu **IP > Firewall** e clique na aba **NAT**.
2.  Clique no botão **+** para adicionar uma nova regra.
3.  Na aba **General**:
    - **Chain:** selecione `srcnat`.
    - **Out. Interface:** selecione a interface que está conectada à internet. Se você usou DHCP, provavelmente é `ether1`. Se usou PPPoE, será a interface `pppoe-out1` .
4.  Vá para a aba **Action**:
    - **Action:** selecione `masquerade`.
5.  Clique em **OK**.

Pronto! Todos os dispositivos conectados às portas LAN agora devem ter acesso à internet.

### 7. Segurança Básica: Protegendo seu Roteador

Um roteador recém-configurado é vulnerável. Siga estas práticas essenciais de segurança:

1.  **Altere a Senha Padrão Imediatamente:**
    - Vá em **System > Users**.
    - Dê um duplo clique no usuário `admin`.
    - Clique no botão **Password...** e defina uma senha forte e difícil de adivinhar .
2.  **Desabilite Serviços Desnecessários:**
    - Vá em **IP > Services**.
    - Você verá uma lista de serviços como `telnet`, `ftp`, `www`, `ssh`, etc.
    - **Desabilite** (selecione e clique em "X") tudo o que você não usa. Por exemplo, se você só usa Winbox, pode desabilitar telnet, ftp e até o www (WebFig), ou restringir o acesso deles apenas à sua rede local (LAN) .
3.  **Mantenha o Sistema Atualizado:**
    - Vá em **System > Packages**.
    - Clique em **Check For Updates**. Se houver uma nova versão estável, é recomendado fazer o download e instalar para corrigir bugs e vulnerabilidades .

### 8. Gerenciamento Básico de Rede

#### 8.1. Controle de Banda (Simple Queue)
Para garantir que um usuário não consuma toda a internet, você pode limitar a velocidade da conexão.
1.  Vá em **Queues > Simple Queues**.
2.  Clique no botão **+**.
3.  Na aba **General**:
    - **Name:** Dê um nome, como "Limite_Usuario1".
    - **Target:** Selecione o endereço IP do usuário (ex: `192.168.88.20/32`).
4.  Na aba **Advanced** (ou **Max Limit** em versões mais antigas), você pode definir os limites de **Target Upload** (velocidade de upload) e **Target Download** (velocidade de download), por exemplo, `2M` para 2 Megabits .

#### 8.2. Backup da Configuração
Sempre faça backup das suas configurações. É um salva-vidas em caso de problemas.
1.  Vá em **Files**.
2.  Clique em **Backup**.
3.  Dê um nome ao arquivo (ex: `backup_meu_roteador`) e clique em **Backup**.
4.  O arquivo `.backup` aparecerá na lista de arquivos. Você pode arrastá-lo para o seu computador para guardá-lo em um local seguro .

### 9. Glossário de Comandos CLI Úteis

Para quem quiser se aventurar na linha de comando (acessando via terminal), aqui estão alguns comandos fundamentais :

| Categoria | Comando | Descrição |
| :--- | :--- | :--- |
| **Sistema** | `/system resource print` | Mostra o uso de CPU, memória e versão do RouterOS. |
| | `/system backup save name=backup` | Cria um backup da configuração. |
| | `/system reboot` | Reinicia o roteador. |
| **Interface** | `/interface print` | Lista todas as interfaces e seus status. |
| | `/interface ethernet monitor-traffic ether1` | Monitora o tráfego em tempo real de uma porta. |
| **IP** | `/ip address print` | Mostra os endereços IP configurados. |
| | `/ip route print` | Exibe a tabela de roteamento. |
| | `/ip dhcp-server lease print` | Lista os clientes que receberam IP do servidor DHCP. |
| | `/ip firewall nat print` | Mostra as regras de NAT configuradas. |
| **Ferramentas** | `/ping 8.8.8.8` | Testa a conectividade com um host remoto. |
| | `/tool traceroute google.com` | Mostra o caminho que os pacotes percorrem até o destino. |

