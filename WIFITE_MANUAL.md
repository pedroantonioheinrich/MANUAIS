## Manual Completo do Wifite: Auditoria e Aplicações Reais em Redes Wi-Fi

### 1. Introdução ao Wifite: O Automatizador de Auditoria Wireless

O **Wifite** é uma ferramenta de auditoria de redes sem fio projetada para automatizar o processo de avaliação de segurança de redes Wi-Fi . Pense nele como um "orquestrador" que consolida o poder de diversas ferramentas clássicas de pentest (como Aircrack-ng, Reaver, Bully, e hcxtools) em um único script Python . Seu principal objetivo é tornar o teste de segurança mais eficiente, eliminando a necessidade de memorizar dezenas de comandos e switches para cada etapa do processo .

**Principais Características:**
- **Automação:** O Wifite gerencia automaticamente a ativação do modo monitor, a varredura de redes, a seleção de alvos e a execução dos ataques mais adequados .
- **Versatilidade:** Suporta uma ampla gama de vetores de ataque contra diferentes tipos de criptografia, incluindo WPA/WPA2, WPS e WEP .
- **Eficiência:** Pode atacar múltiplos alvos simultaneamente e tomar decisões inteligentes com base na potência do sinal e nas vulnerabilidades detectadas .
- **Foco Educacional:** Com a opção `--verbose`, é possível ver exatamente quais comandos estão sendo executados em segundo plano, tornando-o uma excelente ferramenta de aprendizado sobre os fundamentos da segurança wireless .

### 2. Aplicações Reais e Casos de Uso

No contexto de um profissional de segurança, o Wifite não é uma "ferramenta de hackear Wi-Fi", mas sim um **"testador de configurações"**. Seu uso é legitimado em alguns cenários específicos:

#### 2.1. Testes de Intrusão (Pentests) Autorizados
- **Cenário:** Uma empresa contrata um profissional de segurança para avaliar a robustez de sua rede corporativa.
- **Aplicação:** O pentester utiliza o Wifite para tentar capturar handshakes WPA2 ou realizar ataques WPS (Pixie-Dust) na rede da empresa. O objetivo é determinar se um funcionário mal-intencionado ou um visitante na área externa poderia comprometer a rede com ferramentas facilmente acessíveis . Se o Wifite obtiver sucesso, o profissional entrega um relatório recomendando ações como a atualização de firmware, desabilitação do WPS ou adoção de políticas de senha mais fortes.

#### 2.2. Red Team e Simulações de Adversários
- **Cenário:** Uma equipe de Red Team é contratada para simular um ataque realista contra os ativos de uma organização.
- **Aplicação:** O Wifite pode ser usado como uma ferramenta de "ataque inicial". Estacionado em um veículo nas proximidades do escritório, a equipe pode usar o Wifite para tentar invadir a rede Wi-Fi da recepção. Uma vez dentro da rede, eles podem usar outras técnicas para pivotar e tentar acessar sistemas internos mais críticos, testando a capacidade de detecção e resposta da equipe de segurança (Blue Team) .

#### 2.3. Auditoria de Redes Corporativas e Hotspots Públicos
- **Cenário:** Uma empresa de auditoria avalia a segurança de uma filial que oferece Wi-Fi para clientes (hotspot).
- **Aplicação:** A equipe usa o Wifite para verificar se a rede de clientes está adequadamente isolada da rede corporativa interna. Eles podem tentar ataques contra a rede de clientes para ver se é possível "vazar" para a rede administrativa, ou para avaliar a força da criptografia e senha utilizada no acesso público .

#### 2.4. Uso Educacional e Forense
- **Cenário:** Um instrutor de segurança cibernética ensina os conceitos de ataques wireless.
- **Aplicação:** Em um laboratório controlado, com roteadores configurados especificamente para o exercício, o instrutor usa o Wifite para demonstrar, na prática, como um ataque de desautenticação funciona para capturar um handshake WPA2, ou como a vulnerabilidade Pixie-Dusk no WPS pode expor a senha em segundos . Isso ajuda os alunos a entenderem a importância de desabilitar o WPS e usar senhas complexas.

### 3. Pré-requisitos e Preparação do Ambiente

Antes de começar, é crucial garantir que seu hardware e software estejam prontos.

#### 3.1. Hardware Essencial: O Adaptador Wi-Fi
O componente mais crítico é o adaptador wireless. Ele deve suportar dois modos indispensáveis :
- **Modo Monitor (Monitor Mode):** Capacidade de capturar pacotes de redes às quais não está associado.
- **Injeção de Pacotes (Packet Injection):** Capacidade de enviar pacotes forjados na rede, como os quadros de desautenticação.

**Adaptadores Comuns e Recomendados:**
- **Chipsets com bom suporte no Linux:** Modelos que usam chipsets como **Atheros (AR9271)** e **Realtek (RTL8812AU)** são populares e bem suportados .
- **Exemplos físicos:** Adaptadores USB como o **Alfa AWUS036ACH** ou o **TP-Link TL-WN722N (versão v1)** são clássicos no mundo do pentest por sua confiabilidade e compatibilidade .

#### 3.2. Ambiente de Software
- **Sistema Operacional:** O Wifite é nativo do Linux. A distribuição **Kali Linux** é a plataforma preferencial, pois já vem com a ferramenta pré-instalada e todas as dependências necessárias .
- **Dependências:** O Wifite é um invólucro. Ele depende de outras ferramentas instaladas no sistema :
    - **Obrigatórias:** `aircrack-ng` (com `airmon-ng`, `airodump-ng`, `aireplay-ng`).
    - **Recomendadas:** `reaver` (para ataques WPS), `bully` (alternativa para WPS), `tshark` (análise de pacotes), `hashcat` (para cracking avançado de hashes PMKID).

#### 3.3. Instalação
Se não estiver disponível, a instalação é simples no Kali Linux:
```bash
sudo apt update
sudo apt install wifite
```
Este comando geralmente instala o Wifite e suas principais dependências .

### 4. Guia Passo a Passo: Como Usar o Wifite

#### 4.1. Modo Interativo (Mais Comum e Simples)
1.  **Inicie o Wifite com privilégios de root:**
    ```bash
    sudo wifite
    ```
    O script irá automaticamente procurar por interfaces compatíveis, matar processos que possam causar conflito e colocar a placa em modo monitor .

2.  **Varredura de Redes:**
    O Wifite começará a escanear e listar as redes Wi-Fi encontradas. A lista é atualizada em tempo real, mostrando o BSSID (MAC), canal, potência do sinal, criptografia e nome (SSID) da rede .

3.  **Seleção do Alvo:**
    Quando a rede alvo aparecer, pressione **`Ctrl+C`** para interromper a varredura. O Wifite exibirá a lista numerada das redes encontradas e perguntará qual (ou quais) você deseja atacar. Digite o(s) número(s) correspondente(s) e pressione Enter .

4.  **Ataque Automatizado:**
    O Wifite iniciará então uma sequência de ataques pré-definidos. Por padrão, ele tentará primeiro um ataque **PMKID** (mais furtivo) e, se falhar, partirá para a captura de **handshake WPA** com desautenticação .
    - Para WPA/WPA2: Você verá o Wifite enviar pacotes de desautenticação (`deauth`) para forçar a reconexão de um cliente e, consequentemente, a captura do handshake .
    - Para WPS: Ele tentará ataques de PIN online (reaver) e o ataque offline "Pixie-Dust" se a vulnerabilidade existir .

5.  **Cracking e Resultados:**
    - Assim que um handshake for capturado, o Wifite tentará quebrá-lo automaticamente usando um dicionário padrão (`/usr/share/wordlists/rockyou.txt` ou similar) .
    - Se a senha for encontrada, ela será exibida na tela. Todos os handshakes capturados são salvos na pasta `hs/` dentro do diretório de trabalho para análise posterior .

#### 4.2. Principais Opções e Parâmetros (Modo Não-Interativo)
Para usuários mais experientes, o Wifite oferece uma vasta gama de argumentos para personalizar os ataques .

| Parâmetro | Descrição | Exemplo de Uso |
| :--- | :--- | :--- |
| `-i [interface]` | Especifica a interface wireless a ser usada. | `sudo wifite -i wlan0` |
| `-e [essid]` | Ataca uma rede específica pelo nome (SSID). | `sudo wifite -e "RedeAlvo" --dict` |
| `-b [bssid]` | Ataca uma rede específica pelo endereço MAC (BSSID). | `sudo wifite -b 00:11:22:33:44:55` |
| `-pow [db]` | Ataca apenas redes com potência de sinal maior que o valor (ex: -pow 50). | `sudo wifite -pow 60` |
| `--wpa` / `--wep` / `--wps` | Filtra a varredura para mostrar apenas um tipo de rede. | `sudo wifite --wps` |
| `--dict [arquivo]` | Usa um dicionário específico para o cracking do WPA. | `sudo wifite --dict /caminho/wordlist.txt` |
| `--pmkid` | Força o uso exclusivo do ataque PMKID. | `sudo wifite --pmkid` |
| `--pixie` | Força o uso do ataque Pixie-Dust em redes WPS. | `sudo wifite --pixie` |
| `--no-pmkid` | Ignora o ataque PMKID e parte para o deauth. | `sudo wifite --no-pmkid` |
| `-v, --verbose` | Modo verboso. Mostra todos os comandos executados. | `sudo wifite -v` |
| `--cracked` | Exibe uma lista de redes já crackeadas em sessões anteriores. | `sudo wifite --cracked` |

### 5. Cenários de Ataque Detalhados e Aplicações Práticas

Vamos explorar como aplicar o Wifite em situações reais de teste.

#### Cenário 1: Auditoria de Rede WPA2-Personal (Handshake)
- **Objetivo:** Verificar se a senha da rede corporativa "EmpresaWiFi" é vulnerável a um ataque de dicionário.
- **Comando Inicial:** `sudo wifite -e "EmpresaWiFi" --no-pmkid`
    - O `--no-pmkid` força o método tradicional de captura de handshake, garantindo que o ataque funcione mesmo se o AP não suportar PMKID.
- **Processo:** O Wifite escaneará, encontrará a rede e começará a enviar pacotes de desautenticação. Quando um funcionário se reconectar, o handshake será capturado.
- **Resultado:** O Wifite tentará quebrar a senha. Se a senha for "Empresa@2025", ela será encontrada rapidamente se estiver no dicionário. Se for algo complexo como "j?9#fK2!mP8$", o ataque falhará, indicando uma senha forte. O handshake capturado pode ser salvo e passado para ferramentas mais poderosas como o Hashcat para um ataque de força bruta mais sofisticado em um hardware dedicado .

#### Cenário 2: Teste de Vulnerabilidade WPS (Pixie-Dust)
- **Objetivo:** Verificar se os roteadores da filial da empresa são vulneráveis a ataques WPS, que podem expor a senha em minutos.
- **Comando Inicial:** `sudo wifite --wps --pixie`
- **Processo:** O Wifite listará apenas as redes com WPS ativo. Ao selecionar uma, ele executará o ataque "Pixie-Dust", que tenta recuperar o PIN WPS explorando falhas na implementação de alguns fabricantes .
- **Resultado:** Se o roteador for vulnerável (modelos mais antigos ou com firmware desatualizado), o Wifite obterá o PIN e, a partir dele, a senha da rede em segundos. Isso demonstra de forma contundente a necessidade de desabilitar o WPS imediatamente .

#### Cenário 3: Ataque Furtivo PMKID (Sem Desautenticação)
- **Objetivo:** Avaliar a segurança da rede sem causar interrupção para os usuários conectados (ataque passivo).
- **Comando Inicial:** `sudo wifite --pmkid`
- **Processo:** O Wifite escuta o tráfego da rede alvo e tenta capturar o hash PMKID, que é enviado pelo roteador durante a tentativa de associação de um cliente. Este ataque não exige a desautenticação de nenhum usuário .
- **Resultado:** Se o PMKID for capturado, ele é salvo em um formato próprio. O Wifite então tentará quebrá-lo com um dicionário. Este método é o preferido em auditorias onde a discrição é primordial, pois não gera alertas ou interrupções no serviço .

### 6. Análise de Resultados e Pós-Processamento

O trabalho não termina com a captura. Entender os resultados é fundamental.

- **Handshakes Capturados:** Todos os handshakes são salvos na pasta `./hs/` com nomes que incluem o BSSID e a data. O arquivo `.cap` pode ser aberto em ferramentas como o **Wireshark** para análise detalhada do pacote EAPOL .
- **Cracking Manual:** Se o Wifite não conseguir quebrar a senha automaticamente, você pode usar o comando `--crack` para listar os handshakes disponíveis e tentar novamente com um dicionário diferente ou uma ferramenta mais especializada .
    ```bash
    wifite --crack
    ```
    Este comando mostrará uma lista dos handshakes capturados e permitirá que você selecione um para tentar quebrá-lo novamente.
- **Relatórios:** Em um teste profissional, a senha descoberta não é o único entregável. O relatório deve incluir:
    - **Vulnerabilidade Encontrada:** "Rede 'EmpresaWiFi' utiliza senha fraca, vulnerável a ataques de dicionário."
    - **Evidência:** A captura do handshake e o comando que o quebrou.
    - **Risco:** "Um atacante na área externa poderia obter acesso à rede interna, potencialmente levando a vazamento de dados ou ataques secundários."
    - **Recomendação:** "Alterar a senha para uma chave complexa (mínimo 12 caracteres, com letras maiúsculas, minúsculas, números e símbolos) e desabilitar o WPS."

### 7. Aspectos Legais, Éticos e Limitações

**A seção mais importante deste manual.**

- **A LEGALIDADE É INEGOCIÁVEL:** O uso do Wifite em uma rede **sem autorização explícita e por escrito do proprietário** é **ilegal** na grande maioria dos países, configurando crime de invasão de dispositivo informático . As penalidades podem incluir multas severas e prisão. Utilize-o apenas em suas próprias redes ou em redes para as quais você tenha autorização formal para testar.
- **Responsabilidade Ética:** Com grandes poderes vêm grandes responsabilidades. A discrição é vital. Os resultados de um teste de segurança são confidenciais e devem ser tratados como tal.
- **Limitações Técnicas Importantes :**
    1.  **Dependência do Dicionário:** Ataques WPA/WPA2 são, em última análise, ataques de dicionário. Se a senha não estiver na lista de palavras utilizada, o ataque de cracking falhará, mesmo que o handshake tenha sido capturado com sucesso.
    2.  **WPS não é mais uma "bala de prata":** Roteadores fabricados nos últimos anos frequentemente vêm com WPS desabilitado por padrão ou implementam mecanismos de bloqueio após algumas tentativas de PIN inválidas, tornando os ataques online demorados ou impossíveis.
    3.  **WPA3:** O Wifite tem suporte limitado para WPA3. O padrão WPA3 é significativamente mais seguro e, atualmente, não existem métodos práticos e generalizados para quebrar suas senhas usando ferramentas como o Wifite.

