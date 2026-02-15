## Sumário
1.  [O que é o SCCM e Por Que ele Existe?](#1-o-que-é-o-sccm-e-por-que-ele-existe)
2.  [Arquitetura: A Cidade do SCCM](#2-arquitetura-a-cidade-do-sccm)
    - [2.1. Site Server (A Prefeitura)](#21-site-server-a-prefeitura)
    - [2.2. Site System Roles (Os Departamentos)](#22-site-system-roles-os-departamentos)
    - [2.3. Clientes (Os Cidadãos)](#23-clientes-os-cidadãos)
    - [2.4. Limites e Grupos de Limites (Os Bairros)](#24-limites-e-grupos-de-limites-os-bairros)
3.  [Funcionamento Detalhado e Aplicações Reais](#3-funcionamento-detalhado-e-aplicações-reais)
    - [3.1. Descoberta de Recursos](#31-descoberta-de-recursos---como-o-sccm-sabe-que-sua-máquina-existe)
    - [3.2. Inventário de Hardware e Software](#32-inventário-de-hardware-e-software---o-censo-corporativo)
    - [3.3. Distribuição de Software e Aplicações](#33-distribuição-de-software-e-aplicações)
    - [3.4. Atualizações de Software (Patches)](#34-atualizações-de-software-patches---o-posto-de-saúde-digital)
    - [3.5. Implantação de Sistema Operacional (OSD)](#35-implantação-de-sistema-operacional-osd---a-construtora)
    - [3.6. Proteção de Endpoints (Endpoint Protection)](#36-proteção-de-endpoints-endpoint-protection---a-segurança-patrimonial)
    - [3.7. Gerenciamento de Conformidade (Compliance Settings)](#37-gerenciamento-de-conformidade-compliance-settings---o-departamento-de-qualidade)
4.  [Conclusão](#4-conclusão)

---

## 1. O que é o SCCM e Por Que ele Existe?

Imagine que você é o administrador de TI de uma empresa com 5.000 computadores. Como você instala um novo programa (como o pacote Adobe) em todos eles? Se for fazer manualmente, vai a cada computador, insere um USB ou acessa remotamente e executa o instalador. Isso levaria semanas e atrapalharia o trabalho dos usuários.

**O SCCM resolve exatamente esse problema.**

O **Microsoft System Center Configuration Manager (SCCM)** é uma plataforma de gerenciamento abrangente que permite aos administradores de TI gerenciar e controlar um grande número de computadores (estações de trabalho, servidores, dispositivos móveis) de forma centralizada e automatizada . Pense nele como o "prefeito" da sua infraestrutura de TI.

**As principais dores que o SCCM elimina:**
- **Escala:** Fazer manualmente em 10 PCs é fácil, em 1.000 é impossível .
- **Padronização:** Garante que todos os computadores estejam com as mesmas configurações, softwares e versões, cumprindo políticas da empresa.
- **Automação:** Permite que tarefas repetitivas sejam executadas automaticamente, liberando a equipe de TI para projetos mais estratégicos.
- **Inventário Centralizado:** Oferece uma visão única e detalhada de todo o parque de máquinas: quais programas estão instalados, quanto de memória RAM cada uma tem, quais serviços estão rodando, etc. .
- **Segurança e Conformidade:** Ajuda a garantir que todos os sistemas estejam atualizados com os últimos patches de segurança e em conformidade com as políticas internas.

O SCCM não se limita ao Windows; ele também oferece suporte ao gerenciamento de sistemas Linux, Unix e Mac OS X .

---

## 2. Arquitetura: A Cidade do SCCM

Para entender o SCCM, precisamos visualizar sua arquitetura. Vamos usar a analogia de uma **cidade**.

### 2.1. Site Server (A Prefeitura)
É o computador central que detém o poder de decisão. Ele hospeda o banco de dados SQL (que armazena todas as informações) e o console de administração. Existem tipos diferentes de "prefeituras":
- **Central Administration Site (CAS):** O "Governo Federal". Gerencia vários sites primários abaixo dele. Usado em empresas gigantescas com múltiplas filiais.
- **Primary Site:** A "Prefeitura Municipal". É o centro de gerenciamento autônomo. A maioria das empresas opera com um único Primary Site.
- **Secondary Site:** O "Posto Avançado". Controlado por um Primary Site, geralmente usado para gerenciar o tráfego de rede em filiais com conexão lenta.

### 2.2. Site System Roles (Os Departamentos)
A prefeitura sozinha não faz tudo. Ela precisa de departamentos especializados, que são os papéis do sistema.
- **Management Point (MP):** O "Balcão de Informações". É o ponto de contato principal entre os clientes (computadores) e o servidor. Quando um cliente quer saber se há uma nova política ou programa para instalar, ele pergunta ao MP.
- **Distribution Point (DP):** O "Depósito". É onde os arquivos de instalação (o "pacote") são armazenados para que os clientes possam baixá-los. Em vez de 500 computadores baixarem um programa de 1GB do servidor central (sobrecarregando o link), eles baixam do DP mais próximo .
- **Software Update Point (SUP):** O "Posto de Saúde". Integrado ao WSUS (Windows Server Update Services), ele gerencia e distribui os patches de segurança e atualizações da Microsoft.
- **Fallback Status Point (FSP):** O "Hospital". Um ponto especial que monitora o status da instalação do cliente SCCM. Se um computador não consegue se comunicar com o MP, ele pode enviar um sinal de "socorro" para o FSP.

### 2.3. Clientes (Os Cidadãos)
São os computadores gerenciados. Cada um deles tem um pequeno software (o **cliente SCCM**) instalado. Esse software é o "cidadão" que segue as ordens da prefeitura. Ele se comunica periodicamente com o Management Point para saber o que precisa fazer e relatar seu status .

### 2.4. Limites e Grupos de Limites (Os Bairros)
Como a cidade sabe quais cidadãos pertencem a cada região? Através de **Limites** e **Grupos de Limites**.
- **Limite:** Define uma localização na rede. Pode ser um intervalo de IP (ex: 192.168.10.0 - 192.168.10.254), um site do Active Directory ou uma sub-rede IPv6 .
- **Grupo de Limites:** É um conjunto de limites. Você associa um **Distribution Point** a um grupo de limites. **Por quê?** Para que quando um cliente que está no intervalo de IP "192.168.10.x" precise instalar um software, ele procure o DP que está no mesmo grupo de limites, garantindo que a instalação seja rápida e não atravesse links de WAN lentos.

---

## 3. Funcionamento Detalhado e Aplicações Reais

Agora, vamos ver os principais ciclos de vida de um recurso gerenciado pelo SCCM.

### 3.1. Descoberta de Recursos - "Como o SCCM sabe que sua máquina existe?"

**O Conceito:** Antes de gerenciar um computador, o SCCM precisa saber que ele existe. O processo de **Descoberta** cria um registro (chamado **Resource Record**) no banco de dados do SCCM para cada recurso encontrado.

**Como Funciona (O Quê e Por Quê):**
O SCCM usa vários métodos de descoberta, cada um com um propósito específico. O mais comum é o **Active Directory System Discovery**.
1.  **A Ação:** O administrador configura o SCCM para procurar (descobrir) computadores em uma determinada Organizational Unit (OU) do Active Directory.
2.  **A Comunicação:** O servidor SCCM consulta o Active Directory.
3.  **O Resultado:** Ele encontra todos os objetos da classe "Computer" naquela OU e cria um registro para eles no seu banco de dados.
4.  **A Razão:** Agora o SCCM sabe que esses computadores existem. Eles aparecem no console como "Possíveis clientes", mesmo sem o software cliente instalado. Isso permite que o administrador veja a frota potencial e planeje a instalação do cliente.

### 3.2. Inventário de Hardware e Software - "O Censo Corporativo"

**O Conceito:** Uma vez que o cliente está instalado e se comunicando, o SCCM começa a coletar informações detalhadas. Este é o **Inventário** .

**Aplicação Real e Explicação:**
Suponha que um gerente precise saber quantos computadores têm menos de 8GB de RAM para justificar um upgrade antes de instalar um novo sistema.

- **O Ciclo de Inventário:**
    1.  **Coleta:** O agente do SCCM no computador do usuário executa uma tarefa pré-agendada (por padrão, inventário de hardware a cada 7 dias) .
    2.  **O Agente Consulta o WMI:** Ele pergunta ao Windows Management Instrumentation (WMI) do sistema operacional: "Qual é a sua CPU? Quanto de RAM você tem? Qual o tamanho do disco?".
    3.  **Relatório:** O agente empacota essas informações e as envia para o Management Point, que as insere no banco de dados do site.
    4.  **A Consulta:** O administrador abre o console do SCCM e cria uma **consulta** (ou usa um relatório) para listar todos os dispositivos com "TotalPhysicalMemory" menor que 8GB.
- **A Razão para o WMI:** O SCCM não "fuça" diretamente no hardware por questões de segurança e padronização. O WMI é a interface de gerenciamento oficial do Windows, uma camada padronizada que fornece essas informações de forma consistente e segura.

### 3.3. Distribuição de Software e Aplicações

**O Conceito:** O coração do SCCM. É a capacidade de instalar, desinstalar ou atualizar softwares remotamente.

**Aplicação Real:**
A empresa decidiu que todos devem usar o Mozilla Firefox, e não mais o Internet Explorer.

**O Fluxo de Trabalho (Passo a Passo Explicado):**
1.  **Criação do Pacote/Aplicação:** O administrador cria um novo objeto chamado "Aplicação" no console. Ele fornece os arquivos de instalação do Firefox e uma linha de comando de instalação silenciosa (ex: `firefox.exe -ms`).
    - **Por que "silenciosa"?** Para que o usuário final não veja janelas de instalação pop-up e não precise clicar em "Avançar". A experiência é transparente.
2.  **Distribuição de Conteúdo:** O administrador distribui os arquivos de instalação para um ou mais **Distribution Points**.
    - **Por quê?** Se a empresa tem filiais no Rio e em São Paulo, ele distribui o pacote para DPs nessas filiais. Quando um computador no Rio for instalar, ele baixará do DP do Rio, não do servidor central em São Paulo.
3.  **Criação da Coleção:** O administrador cria uma **Coleção** (um grupo de recursos). Pode ser uma coleção direta ("Todos os computadores do Setor Financeiro") ou uma coleção baseada em consulta ("Todos os computadores que não possuem o Firefox instalado").
4.  **Implantação (Deployment):** O administrador "implanta" a aplicação do Firefox na coleção criada. Ele escolhe o **Propósito** da implantação:
    - **Obrigatório:** A instalação acontecerá automaticamente no próximo prazo agendado. O usuário pode adiar, mas não pode cancelar.
    - **Disponível:** A aplicação aparece no **Software Center** do computador do usuário . O usuário pode clicar em "Instalar" quando quiser, sem precisar de privilégios de administrador local.
5.  **Ação do Cliente:** O agente SCCM no computador do usuário, a cada 60 minutos, pergunta ao Management Point: "Tem política nova para mim?" . O MP responde: "Sim, você deve instalar o Firefox, e os arquivos estão no DP Rio".
6.  **Download e Instalação:** O cliente baixa os arquivos do DP e executa o comando de instalação silenciosa.
7.  **Relatório de Status:** Após a instalação (sucesso ou falha), o cliente reporta o resultado de volta ao Management Point. O administrador pode monitorar o progresso em tempo real.

### 3.4. Atualizações de Software (Patches) - "O Posto de Saúde Digital"

**O Conceito:** Gerenciar os patches de segurança do Windows e outros produtos Microsoft de forma centralizada.

**Aplicação Real:**
A Microsoft lançou a "Patch Tuesday" (segunda terça-feira do mês). O administrador precisa garantir que todos os servidores críticos estejam atualizados até o final da semana, mas sem causar indisponibilidade.

**O Fluxo de Trabalho:**
1.  **Sincronização:** O SCCM (através do papel de **Software Update Point**) sincroniza com o Microsoft Update, baixando os metadados (informações sobre as atualizações) de todos os novos patches .
2.  **Avaliação:** Os clientes SCCM avaliam quais patches estão faltando e reportam essa informação de volta.
3.  **Criação do Grupo de Atualizações:** O administrador cria um "Grupo de Atualizações de Software" com os patches críticos para aquele mês e cria uma "Regra de Implantação Automática" (ADR).
4.  **Implantação em Etapas (Manutenção):** O administrador implanta o grupo na coleção "Servidores de Produção", mas com uma configuração especial: **"Hora de Instalação" configurada para uma janela de madrugada (ex: 2 da manhã)** e com **"Prazo de Instalação"** definido como **"Assim que possível após o prazo"**.
    - **A Razão das Etapas:** Ele cria duas etapas na implantação. A primeira etapa instala em 10% dos servidores na quarta-feira para testar. Se não houver problemas, a segunda etapa instala automaticamente nos 90% restantes no sábado de madrugada. Isso minimiza o risco de uma atualização defeituosa parar toda a operação.

### 3.5. Implantação de Sistema Operacional (OSD) - "A Construtora"

**O Conceito:** É o processo de instalar ou migrar o sistema operacional (Windows 10/11) em máquinas novas (bare-metal) ou substituir o sistema de máquinas antigas .

**Aplicação Real:**
A empresa comprou 100 computadores novos e precisa colocar a imagem padrão da empresa (com Windows 10, Office, antivírus e sistemas internos) neles. Fazer um a um com um pen drive levaria uma eternidade.

**O Fluxo de Trabalho:**
1.  **Criação da Imagem de Referência:** O administrador pega um computador modelo, instala o Windows, todos os softwares e configurações necessárias. Depois, ele usa uma **Sequência de Tarefas (Task Sequence)** do SCCM para "capturar" essa instalação, gerando um arquivo de imagem (arquivo `.wim`).
2.  **Distribuição da Imagem:** Essa imagem `.wim` é distribuída para os Distribution Points.
3.  **Configuração do PXE:** O administrador habilita o PXE (Preboot Execution Environment) em um Distribution Point.
    - **O que é PXE?** É um protocolo que permite que um computador sem sistema operacional (bare-metal) boote pela placa de rede e busque um sistema para iniciar a instalação.
4.  **O Milagre Acontece:**
    - O funcionário liga o computador novo. Ele está configurado para boot via rede (PXE).
    - O computador envia um broadcast na rede pedindo um servidor PXE.
    - O DP responde e envia um pequeno "boot image" (baseado no Windows PE).
    - O computador carrega essa imagem na memória e se conecta ao Management Point.
    - Ele se identifica e o SCCM diz: "Você é novo. Aqui está a Task Sequence para instalar sua máquina."
    - O computador baixa a imagem `.wim` do DP, aplica no disco rígido, instala drivers, e se junta ao domínio. Tudo isso sem intervenção humana .

### 3.6. Proteção de Endpoints (Endpoint Protection) - "A Segurança Patrimonial"

**O Conceito:** Gerenciar o antivírus e firewall do Windows (Windows Defender) centralizadamente.

**Aplicação Real:**
Um usuário, por engano, tenta acessar um site malicioso que tenta baixar um malware.

- **O Fluxo:**
    1.  O administrador criou uma política de antimalware no SCCM e a implantou para todos os clientes.
    2.  Essa política força o Windows Defender a estar sempre ativo, com as definições de vírus mais recentes e com a proteção em tempo real ligada.
    3.  Quando o usuário tenta acessar o site, o Windows Defender (obedecendo à política do SCCM) bloqueia o acesso imediatamente e registra a tentativa.
    4.  O administrador pode configurar alertas para ser notificado se vários computadores da empresa começarem a detectar o mesmo malware, indicando um ataque direcionado.

### 3.7. Gerenciamento de Conformidade (Compliance Settings) - "O Departamento de Qualidade"

**O Conceito:** Garantir que os computadores estejam configurados de acordo com as políticas de segurança da empresa (ex: senha de tela bloqueada deve ser exigida, firewall deve estar ligado, determinados serviços devem estar desabilitados).

**Aplicação Real:**
A auditoria interna exige que o serviço de "Área de Trabalho Remota" (Remote Desktop) esteja desabilitado em todas as estações de trabalho por questões de segurança.

**O Fluxo de Trabalho:**
1.  **Criação do Item de Configuração:** O administrador cria um **Item de Configuração** que verifica o estado do serviço "Remote Desktop" no registro do Windows.
2.  **Criação da Linha de Base:** Ele cria uma **Configuration Baseline** que contém esse Item de Configuração, definindo que o valor correto é "Disabled".
3.  **Implantação:** Ele implanta essa Baseline na coleção "Todas as Estações de Trabalho".
4.  **Avaliação e Remediação:**
    - Os clientes avaliam sua conformidade.
    - Um usuário, que é administrador local da sua máquina, tinha ativado a Área de Trabalho Remota.
    - O cliente SCCM reporta que está "Não Conforme".
    - O SCCM, dependendo da configuração, pode **automaticamente remediar** o problema, desabilitando o serviço novamente.
    - O administrador pode gerar um relatório mostrando que 99,9% das máquinas estão conformes, exceto aquela do usuário.

