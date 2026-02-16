
## **Manual de Diagnóstico de Rede para Linux Mint: `gping` e `mtr`**

Este manual tem como objetivo fornecer um guia completo para o uso de duas ferramentas essenciais para diagnóstico de desempenho de rede no Linux Mint: `gping` e `mtr`. Enquanto o `ping` tradicional é útil para uma verificação rápida de conectividade, estas ferramentas oferecem uma visão mais aprofundada e dinâmica, permitindo uma análise mais precisa de latência, perda de pacotes e do caminho que os dados percorrem até o destino.


### **Parte 1: `gping` - O Ping com Gráfico**

O `gping` é uma ferramenta moderna que transforma a saída tradicional do `ping` em um gráfico interativo em tempo real diretamente no seu terminal. Em vez de apenas linhas de texto com o tempo de resposta, você vê uma evolução gráfica desse tempo, o que facilita a identificação de padrões, picos de latência e instabilidades.

#### **1.1 Instalação no Linux Mint**

No Linux Mint, que é baseado no Ubuntu, a instalação pode ser feita de duas maneiras: baixando o binário diretamente ou compilando pelo código-fonte (Rust). O método mais simples é baixar o binário compilado.

**Método 1: Download do Binário (Recomendado)**
1.  Acesse a página de lançamentos do projeto `gping` no GitHub: [https://github.com/orf/gping/releases](https://github.com/orf/gping/releases)
2.  Procure pelo arquivo mais recente compatível com Linux, geralmente nomeado como `gping-Linux-x86_64.tar.gz` (para sistemas 64 bits).
3.  Faça o download e extraia o conteúdo. Você pode fazer isso pelo terminal:
    ```bash
    # Substitua o link pela versão mais atual encontrada no site
    wget https://github.com/orf/gping/releases/download/v1.10.0/gping-Linux-x86_64.tar.gz
    tar -xzf gping-Linux-x86_64.tar.gz
    ```
4.  Dê permissão de execução ao binário e mova-o para um diretório que esteja no seu `PATH` (como `/usr/local/bin`) para poder executá-lo de qualquer lugar:
    ```bash
    chmod +x gping-Linux-x86_64/gping
    sudo mv gping-Linux-x86_64/gping /usr/local/bin/
    ```

**Método 2: Instalação via Cargo (Rust)**
Se você possui o gerenciador de pacotes do Rust (Cargo) instalado, pode compilar e instalar diretamente:
```bash
cargo install gping
```

#### **1.2 Como Usar e Principais Opções**

A sintaxe básica é `gping [OPÇÕES] [HOST]`. Se executado sem opções, ele começará a pingar e mostrar o gráfico do host especificado.

A tabela abaixo detalha as principais opções disponíveis:

| Opção | Descrição | Exemplo |
| :--- | :--- | :--- |
| `--cmd` | **Superpoder do `gping`.** Em vez de pingar um host, ele mede e gráfica o tempo de execução de um ou mais comandos. Útil para comparar desempenho de scripts ou programas. | `gping --cmd "curl https://site1.com" "curl https://site2.com"` |
| `-n, --watch-interval` | Define o intervalo (em segundos) entre as medições de um comando (usado com `--cmd`). | `gping --cmd "meu_script.sh" -n 2` |
| `-d, --data` | Exibe pontos de dados adicionais (timestamp) no gráfico. | `gping -d google.com` |
| `-s, --simple` | Usa um estilo de gráfico mais simples e sem cores (útil para terminais antigos). | `gping -s google.com` |
| `--clear` | Limpa a tela do gráfico a cada novo frame (padrão é sobrescrever). | `gping --clear google.com` |
| `-4` | Usa apenas IPv4. | `gping -4 google.com` |
| `-6` | Usa apenas IPv6. | `gping -6 google.com` |

#### **1.3 Análise dos Gráficos do `gping`**

O `gping` exibe um gráfico de linhas onde:
*   **Eixo X (horizontal):** Representa o tempo (as amostras mais recentes ficam à direita).
*   **Eixo Y (vertical):** Representa o tempo de resposta (latência) em milissegundos (ms).

**Como interpretar:**
*   **Linha Estável:** Uma linha reta e horizontal indica uma conexão estável, com latência constante.
*   **Picos (Spikes):** Picos súbitos no gráfico indicam que um ou mais pacotes demoraram mais que o normal para responder. Isso pode ser causado por congestionamento momentâneo na rede, bufferbloat ou problemas no roteador intermediário.
*   **Linha com Oscilações Constantes:** Se a linha do gráfico sobe e desce frequentemente (jitter), significa que a conexão é instável, o que pode prejudicar aplicações em tempo real como jogos e chamadas de vídeo.
*   **Comparação de Múltiplos Hosts:** Usando o `gping` com vários argumentos (ex: `gping google.com cloudflare.com`), você pode comparar a estabilidade e latência de diferentes destinos lado a lado no mesmo gráfico, cada um com sua cor.

---

### **Parte 2: `mtr` (My Traceroute) - O Mapa do Caminho da Rede**

O `mtr` é uma ferramenta poderosa que combina a funcionalidade do `traceroute` (mostrar a rota) com a do `ping` (medir a qualidade da conexão em cada salto). Ela envia pacotes continuamente e fornece estatísticas em tempo real de cada roteador (hop) entre o seu computador e um destino .

#### **2.1 Instalação no Linux Mint**

No Linux Mint, o `mtr` está nos repositórios oficiais, o que torna a instalação muito simples .

1.  Abra um terminal (Ctrl + Alt + T).
2.  Atualize a lista de pacotes e instale o `mtr`:
    ```bash
    sudo apt update
    sudo apt install mtr-tiny
    ```
    *Nota: O pacote `mtr-tiny` é uma versão mais leve e sem dependências gráficas desnecessárias, ideal para servidores e uso no terminal. O pacote `mtr` também funciona e inclui uma interface gráfica (GTK) que pode ser aberta com o comando `xmtr`.*

#### **2.2 Como Usar e Principais Opções**

A sintaxe básica é `mtr [OPÇÕES] DESTINO` .

| Opção | Descrição | Exemplo |
| :--- | :--- | :--- |
| `-r, --report` | **Modo de relatório.** Após enviar um número predefinido de pacotes (padrão 10), o `mtr` exibe um resumo estatístico e encerra. Ideal para gerar relatórios para análise ou suporte técnico . | `mtr -r google.com` |
| `-c, --report-cycles` | Define o número de pacotes a serem enviados no modo de relatório (usado com `-r`) . | `mtr -r -c 50 google.com` |
| `-n, --no-dns` | Não resolve nomes de domínio, mostrando apenas endereços IP. Isso acelera o processo e é útil quando o servidor DNS está lento ou com problemas . | `mtr -n google.com` |
| `-w, --report-wide` | Usa um formato de saída mais largo, exibindo o nome completo do host . | `mtr -rw google.com` |
| `-i, --interval` | Define o intervalo (em segundos) entre o envio de cada pacote . | `mtr -i 0.5 google.com` |
| `-4` | Força o uso de IPv4 . | `mtr -4 google.com` |
| `-6` | Força o uso de IPv6 . | `mtr -6 google.com` |
| `--tcp` | Usa pacotes TCP (SYN) em vez do padrão ICMP. Útil para testar conectividade de serviços específicos ou quando firewalls bloqueiam ICMP . | `sudo mtr --tcp --port 443 google.com` |
| `--udp` | Usa pacotes UDP em vez de ICMP . | `sudo mtr --udp google.com` |

#### **2.3 Entendendo as Colunas da Saída do `mtr`**

Quando você executa `mtr`, verá uma tabela semelhante a esta :

```bash
 HOST: LocalHost                   Loss%   Snt   Last   Avg  Best  Wrst  StDev
 1. 192.168.1.1                    0.0%    10   2.1    2.3   1.8   2.8   0.3
 2. 10.10.0.1                       0.0%    10   5.2    5.4   5.0   6.1   0.4
 3. 181.213.132.1                   0.0%    10   21.3   20.9  19.8  23.1   1.0
 4. 72.14.215.68                    0.0%    10   22.1   22.4  20.9  24.3   1.1
 5. 209.85.252.72                   0.0%    10   23.5   22.9  21.7  24.8   0.9
 6. lga25s02-in-f14.1e100.net       0.0%    10   22.8   23.2  21.9  25.4   1.1
```

| Coluna | Descrição | O que significa? |
| :--- | :--- | :--- |
| **Host** | O nome do domínio ou endereço IP do roteador naquele salto (hop). Se aparecer `???`, significa que o roteador não respondeu (pode ser por configuração ou firewall) . | Identifica o ponto na rede por onde os dados passam. |
| **Loss%** | A porcentagem de perda de pacotes naquele salto específico . | **O indicador mais crítico.** Valores acima de 0% podem indicar um problema, mas é preciso analisar o contexto (veja seção de análise). |
| **Snt** | O número total de pacotes enviados até aquele ponto . | Confirma a amostragem para as estatísticas de perda e latência. |
| **Last** | A latência (tempo de ida e volta) do último pacote enviado, em milissegundos (ms) . | Mostra o comportamento mais recente da rede. |
| **Avg** | A latência média de todos os pacotes enviados para aquele host . | A métrica de latência mais importante para avaliar o desempenho geral. |
| **Best** | A menor latência registrada (melhor caso) . | Representa o desempenho em condições ideais. |
| **Wrst** | A maior latência registrada (pior caso) . | Ajuda a identificar picos de lentidão e inconsistência. |
| **StDev** | O desvio padrão da latência. Quanto maior o número, mais a latência variou (maior o jitter) . | Indica a **estabilidade** da conexão naquele salto. Valores baixos são bons. |

#### **2.4 Análise de Resultados do `mtr` - Guia Passo a Passo**

Para diagnosticar um problema com o `mtr`, siga esta lógica :

**Passo 1: Identifique o Tipo de Problema**
*   **Lentidão:** Foco nas colunas `Avg` e `StDev`.
*   **Desconexões/Instabilidade:** Foco na coluna `Loss%`.

**Passo 2: Analise a Perda de Pacotes (Loss%) - O Ponto Mais Importante**
A perda de pacotes raramente é boa. Mas é crucial saber **onde** ela começa.
*   **Cenário A: Perda no Primeiro Salto (seu roteador).** O problema é na sua rede local (cabo, Wi-Fi, placa de rede, roteador). Reinicie o roteador e verifique seus cabos.
*   **Cenário B: Perda em um Salto Intermediário, mas não nos Saltos Seguintes.**
    ```
     3. 181.213.132.1                   20.0%    10   21.3   20.9  19.8  23.1   1.0
     4. 72.14.215.68                    0.0%    10   22.1   22.4  20.9  24.3   1.1
    ```
    *   **Análise:** O salto 3 mostra perda, mas o salto 4 não. Isso geralmente significa que o roteador no salto 3 está configurado para não priorizar ou responder aos pacotes ICMP do `mtr` (algo comum em roteadores de borda de operadoras para reduzir carga). **Isso NÃO indica um problema real de perda de pacotes no caminho** .
*   **Cenário C: Perda em um Salto e Persistência até o Fim.**
    ```
     3. 181.213.132.1                   0.0%    10   21.3   20.9  19.8  23.1   1.0
     4. 72.14.215.68                    20.0%    10   22.1   22.4  20.9  24.3   1.1
     5. 209.85.252.72                   20.0%    10   23.5   22.9  21.7  24.8   0.9
     6. lga25s02-in-f14.1e100.net       20.0%    10   22.8   23.2  21.9  25.4   1.1
    ```
    *   **Análise:** A perda começa no salto 4 e continua em todos os seguintes. Isso indica que o problema (congestionamento, falha de link) está no enlace entre os saltos 3 e 4, ou no próprio roteador do salto 4. O tráfego está sendo descartado a partir dali .

**Passo 3: Analise a Latência (Avg, StDev)**
*   **Aumento Gradual:** É normal a latência aumentar um pouco a cada salto, pois a distância geográfica aumenta.
*   **Aumento Súbito e Grande:**
    ```
     2. 10.10.0.1                       0.0%    10   5.2    5.4   5.0   6.1   0.4
     3. 181.213.132.1                   0.0%    10   70.3   71.9  68.8  75.1   2.0
    ```
    *   **Análise:** Um salto de 5ms para 70ms é um forte indicador de um link congestionado, rota geograficamente longa (ex: saindo do país) ou um roteador com problemas de processamento.
*   **StDev Alto:** Um valor alto de desvio padrão, como 10, 20 ou mais, indica que a latência varia muito (jitter). Isso é péssimo para aplicações em tempo real .

**Passo 4: Ações Baseadas na Análise**
*   **Problema na Rede Local (primeiros saltos):** Reinicie o roteador/modem. Teste com cabo em vez de Wi-Fi. Verifique a placa de rede.
*   **Problema na Operadora (saltos intermediários):** Reúna 3 ou 4 relatórios do `mtr` (use `mtr -rwc 100 destino > relatorio.txt`) para diferentes horários do dia e locais (ex: google.com, seu servidor, etc.). Envie-os para o suporte técnico da sua operadora. Eles terão dados concretos para investigar.
*   **Problema no Servidor de Destino (saltos finais):** Se a perda ou latência alta só aparece nos últimos saltos, o problema pode estar no servidor ou na rede do data center dele. Contate o suporte do serviço ou hosting.

### **Resumo para o Dia a Dia**

1.  Para ver a **estabilidade da sua conexão** de forma rápida e visual: `gping google.com`
2.  Para **descobrir onde está o gargalo** (lentidão ou perda de pacotes): `mtr -rw google.com` (depois analise a tabela com calma).
3.  Para **gerar um relatório** para enviar a um técnico ou guardar: `mtr -rwc 100 google.com > relatorio_mtr.txt`
