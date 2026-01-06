# 📡 Manual Completo: Wavemon - Monitor de Redes Wireless

```
Autor: [Seu Nome]
Data: [Data Atual]
Versão: 1.2
Descrição: Monitoramento abrangente de interfaces wireless
Licença: GPL v3
```

## 🎯 Introdução ao Wavemon

### O que é o Wavemon?
Wavemon é um **monitor de interface de rede wireless** para Linux, com interface baseada em ncurses que permite monitorar em tempo real o status, estatísticas e parâmetros de redes Wi-Fi.

### Características Principais
- ✅ Interface TUI (Text User Interface) intuitiva
- ✅ Monitoramento em tempo real de sinal e ruído
- ✅ Informações detalhadas da interface wireless
- ✅ Suporte a múltiplos modos de visualização
- ✅ Configuração de parâmetros da interface
- ✅ Scan de redes disponíveis
- ✅ Histograma de qualidade de sinal

### Casos de Uso Comuns
- **Diagnóstico de problemas** de conexão Wi-Fi
- **Otimização de posicionamento** de dispositivos
- **Comparação de qualidade** entre diferentes redes
- **Monitoramento contínuo** de estabilidade da conexão
- **Análise de segurança** de redes wireless

## 📦 Capítulo 1: Instalação

### 1.1 Instalação em Distribuições Baseadas em Debian/Ubuntu
```bash
# Método recomendado via repositório oficial
sudo apt update
sudo apt install wavemon

# Instalação com todas as dependências
sudo apt install wavemon wireless-tools iw wpasupplicant

# Instalação da versão de desenvolvimento (opcional)
sudo apt install build-essential libncurses5-dev libncursesw5-dev
git clone https://github.com/uoaerg/wavemon.git
cd wavemon
./autogen.sh
./configure
make
sudo make install
```

### 1.2 Instalação em Distribuições RHEL/Fedora
```bash
# Fedora
sudo dnf install wavemon

# RHEL/CentOS 8+
sudo dnf install epel-release
sudo dnf install wavemon

# Ou via compilação
sudo dnf install gcc make ncurses-devel
wget https://github.com/uoaerg/wavemon/releases/download/v0.9.4/wavemon-0.9.4.tar.gz
tar -xzf wavemon-0.9.4.tar.gz
cd wavemon-0.9.4
./configure
make
sudo make install
```

### 1.3 Instalação em Arch Linux
```bash
# Via pacman
sudo pacman -S wavemon

# Ou AUR (versão de desenvolvimento)
yay -S wavemon-git
```

### 1.4 Verificação da Instalação
```bash
# Verificar versão instalada
wavemon -v

# Verificar dependências
ldd $(which wavemon)

# Testar execução básica
wavemon -h
```

### 1.5 Dependências e Requisitos
```bash
# Bibliotecas essenciais
- libncurses5-dev ou ncurses-devel
- wireless-tools (iwconfig, iwlist)
- iw (ferramentas wireless modernas)

# Kernel e drivers
- Módulos wireless carregados
- Suporte a nl80211 no kernel
- Drivers wireless compatíveis

# Verificar suporte wireless
lsmod | grep -E "(cfg80211|mac80211|iwlwifi|ath)"
```

## 🚀 Capítulo 2: Primeiros Passos

### 2.1 Execução Básica
```bash
# Executar com interface padrão
sudo wavemon

# Especificar interface wireless
sudo wavemon -i wlan0

# Executar em modo não-interativo (para scripts)
sudo wavemon -i wlan0 -q

# Executar com intervalo de atualização personalizado
sudo wavemon -r 2  # Atualizar a cada 2 segundos
```

### 2.2 Interface Gráfica - Visão Geral
```
┌─────────────────────────────────────────────────────────────────────┐
│ Wavemon v0.9.4  -  Monitoring wlan0                                 │
├─────────────────────────────────────────────────────────────────────┤
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐        │
│ │ Level   │ │ Info    │ │ Scan    │ │ Prefs   │ │ Help    │        │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘        │
│                                                                     │
│ ESSID: MyHomeWiFi                  Mode: Managed                    │
│ AP: AA:BB:CC:DD:EE:FF             Frequency: 5.180 GHz              │
│ Bitrate: 866.7 Mb/s               Tx-Power: 22 dBm                  │
│                                                                     │
│ Signal Level: -42 dBm              Noise Level: -92 dBm             │
│ Quality: 84/100                    Link: 86/100                     │
│                                                                     │
│ ┌─────────────────────────────────────────────────────────────────┐ │
│ │ ▁▂▃▅▆▇▇▆▅▃▂▁▁▂▃▅▆▇▇▆▅▃▂▁  Histogram (last 60s)                   │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
│ Packets: 142.5k ▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏▏ │
│                                                                     │
│ [q]uit [h]elp [F1]-[F5] switch views                               │
└─────────────────────────────────────────────────────────────────────┘
```

### 2.3 Navegação Básica
```
Teclas de Navegação:
  F1-F5       - Alternar entre abas/visões
  Tab         - Alternar entre elementos
  Setas       - Navegar em listas
  Enter       - Selecionar/Confirmar
  Espaço      - Marcar/Desmarcar (em scan)
  q           - Sair do programa
  h           - Ajuda contextual
  ?           - Ajuda geral
  
Atalhos por Aba:
  Level View  - 'l' ou F1
  Info View   - 'i' ou F2
  Scan View   - 's' ou F3
  Prefs View  - 'p' ou F4
  Help View   - 'h' ou F5
```

### 2.4 Modo de Execução Automático
```bash
# Para monitoramento contínuo em background
#!/bin/bash
# monitor-wifi.sh
INTERFACE="wlan0"
LOG_FILE="/var/log/wifi-monitor-$(date +%Y%m%d).log"

echo "Iniciando monitoramento de $INTERFACE em $(date)" >> "$LOG_FILE"
while true; do
    echo "=== $(date) ===" >> "$LOG_FILE"
    wavemon -i "$INTERFACE" -q -c 1 >> "$LOG_FILE"
    sleep 30
done

# Executar com:
# chmod +x monitor-wifi.sh
# sudo ./monitor-wifi.sh &
```

## 📊 Capítulo 3: Abas e Funcionalidades

### 3.1 Level View (F1) - Monitoramento de Sinal
```
┌─────────────────────────────────────────────────────┐
│ Level View - wlan0                                  │
├─────────────────────────────────────────────────────┤
│ ESSID: HomeNetwork              Frequency: 2.437GHz │
│ Signal: -55 dBm                                      │
│ Noise:  -92 dBm                                      │
│ Quality: ████████████████████ 65/100                │
│ Link:    ████████████████████████ 78/100            │
│                                                     │
│ Histogram (últimos 60 segundos):                    │
│  -60 ┤  ▁▂▃▄▅▆▇█▇▆▅▄▃▂▁                              │
│  -65 ┤ ▁▂▃▄▅▆▇████▇▆▅▄▃▂▁                            │
│  -70 ┤▁▂▃▄▅▆▇███████▇▆▅▄▃▂▁                          │
│                                                     │
│ Estatísticas:                                       │
│  ↓ 1.2 MB/s    ↑ 350 KB/s                           │
│  ↓ 145 pkt/s   ↑ 89 pkt/s                           │
└─────────────────────────────────────────────────────┘
```

#### Elementos da Level View:
- **Signal Level**: Intensidade do sinal em dBm (quanto mais próximo de 0, melhor)
- **Noise Level**: Nível de ruído em dBm (quanto mais negativo, melhor)
- **Quality**: Qualidade do sinal (0-100)
- **Link**: Qualidade do link (0-100)
- **Histogram**: Variação do sinal ao longo do tempo
- **Throughput**: Taxa de transferência (download/upload)
- **Packet Rate**: Pacotes por segundo

### 3.2 Info View (F2) - Informações da Interface
```
┌─────────────────────────────────────────────────────┐
│ Info View - wlan0                                   │
├─────────────────────────────────────────────────────┤
│ General Information:                                │
│   Interface: wlan0 (phy0)                          │
│   Driver: iwlwifi                                  │
│   Chipset: Intel(R) Wi-Fi 6 AX200                  │
│                                                     │
│ Current Connection:                                 │
│   ESSID: HomeNetwork                               │
│   BSSID: AA:BB:CC:DD:EE:FF                         │
│   Mode: Managed                                    │
│   Channel: 6 (2.437 GHz)                           │
│   Width: 40 MHz                                    │
│                                                     │
│ Signal Statistics:                                 │
│   Avg Signal: -58 dBm                              │
│   Avg Noise:  -91 dBm                              │
│   Max Signal: -42 dBm                              │
│   Min Signal: -72 dBm                              │
│                                                     │
│ Network Stats:                                     │
│   TX Bytes:   145.2 MB                             │
│   RX Bytes:   892.4 MB                             │
│   TX Packets: 234,567                              │
│   RX Packets: 1,234,567                            │
│   Errors:     12                                   │
│   Drops:      3                                    │
└─────────────────────────────────────────────────────┘
```

#### Informações Disponíveis:
- **Identificação**: Nome da interface, driver, chipset
- **Conexão**: ESSID, BSSID, modo, canal, frequência
- **Estatísticas de Sinal**: Média, máximo, mínimo
- **Estatísticas de Rede**: Bytes, pacotes, erros
- **Configurações**: Potência TX, bitrate, modo
- **Capacidades**: Protocolos suportados, cifras

### 3.3 Scan View (F3) - Scan de Redes
```
┌─────────────────────────────────────────────────────────────────┐
│ Scan View - Networks available                                  │
├─────────────────────────────────────────────────────────────────┤
│ Nr. ESSID              BSSID              Chan Sig Encr  Mode  │
├─────────────────────────────────────────────────────────────────┤
│ ▶ 1 HomeNetwork        AA:BB:CC:DD:EE:FF    6  -54 WPA2  Infra │
│   2 GuestNet           CC:DD:EE:FF:AA:BB    1  -62 WPA2  Infra │
│   3 NETGEAR23          11:22:33:44:55:66   11  -72 WPA   Infra │
│   4 AndroidAP          FF:EE:DD:CC:BB:AA    6  -48 Open  AP    │
│   5 xfinitywifi        00:11:22:33:44:55    6  -65 Open  Infra │
│   6 Dlink_5G           AA:11:BB:22:CC:33   36  -62 WPA2  Infra │
│                                                     │
│ Selected Network:                                    │
│   ESSID: HomeNetwork                                 │
│   Channel: 6 (2.437 GHz)                            │
│   Signal: -54 dBm                                    │
│   Encryption: WPA2 Personal                          │
│   Mode: Infrastructure                               │
│                                                     │
│ Actions: [S]can  [C]onnect  [R]efresh              │
└─────────────────────────────────────────────────────┘
```

#### Funcionalidades do Scan:
- **Lista completa** de redes disponíveis
- **Ordenação** por sinal, canal, ou nome
- **Filtros** por tipo de segurança
- **Informações detalhadas** da rede selecionada
- **Ações**: Scan, conectar, atualizar

#### Comandos na Scan View:
```bash
Teclas:
  S          - Iniciar novo scan
  C          - Conectar à rede selecionada
  R          - Atualizar lista
  Espaço     - Selecionar/deselecionar rede
  Setas      - Navegar na lista
  Enter      - Ver detalhes da rede
  /          - Buscar rede por nome
  F          - Filtrar por segurança
```

### 3.4 Prefs View (F4) - Preferências
```
┌─────────────────────────────────────────────────────┐
│ Preferences View                                    │
├─────────────────────────────────────────────────────┤
│ Interface Settings:                                 │
│   [ ] Auto-select interface                         │
│   [x] Monitor mode detection                        │
│   [ ] Use SI units for rates                        │
│                                                     │
│ Display Settings:                                   │
│   Refresh interval: ████████ 2.0 seconds            │
│   Histogram length: ████████ 60 samples             │
│   [x] Show histogram                                │
│   [x] Show quality bars                             │
│   [ ] Extended info mode                            │
│                                                     │
│ Color Settings:                                     │
│   Signal color: Green                               │
│   Noise color:  Red                                 │
│   Quality color: Cyan                               │
│                                                     │
│ [S]ave  [L]oad  [D]efaults                         │
└─────────────────────────────────────────────────────┘
```

#### Configurações Disponíveis:
- **Interface**: Auto-seleção, detecção de modo monitor
- **Display**: Intervalo de atualização, tamanho do histograma
- **Cores**: Esquema de cores personalizável
- **Unidades**: SI ou binárias para taxas de transferência
- **Logging**: Habilitar logging, definir arquivo de log

### 3.5 Help View (F5) - Ajuda
```
┌─────────────────────────────────────────────────────┐
│ Help View                                           │
├─────────────────────────────────────────────────────┤
│ General Keys:                                       │
│   q, Ctrl+C  - Quit                                │
│   h, F5      - This help screen                    │
│   ?          - Show key bindings                   │
│                                                     │
│ View Switching:                                     │
│   F1, l      - Level view                          │
│   F2, i      - Info view                           │
│   F3, s      - Scan view                           │
│   F4, p      - Preferences view                    │
│                                                     │
│ Level View:                                         │
│   r          - Reset statistics                    │
│   c          - Clear histogram                     │
│                                                     │
│ Scan View:                                          │
│   S          - Start new scan                      │
│   C          - Connect to selected network         │
│   R          - Refresh network list                │
│   /          - Search networks                     │
│                                                     │
│ [M]an pages  [O]nline docs  [A]bout               │
└─────────────────────────────────────────────────────┘
```

## ⚙️ Capítulo 4: Uso Avançado

### 4.1 Opções de Linha de Comando
```bash
# Sintaxe completa
wavemon [OPÇÕES] [INTERFACE]

# Opções principais
-w, --watch        Modo de monitoramento contínuo
-i, --interface    Especificar interface wireless
-r, --refresh      Intervalo de atualização em segundos
-l, --level        Iniciar na aba Level
-s, --scan         Iniciar na aba Scan
-c, --count        Número de iterações antes de sair
-q, --quiet        Modo não-interativo (para scripts)
-v, --version      Mostrar versão
-h, --help         Mostrar ajuda

# Exemplos avançados
# Monitorar com intervalo de 5 segundos
sudo wavemon -i wlan0 -r 5

# Executar 10 ciclos de monitoramento e sair
sudo wavemon -i wlan0 -c 10 -q

# Iniciar diretamente no modo scan
sudo wavemon -i wlan0 -s

# Salvar saída em arquivo
sudo wavemon -i wlan0 -q -c 5 > wifi-stats.txt
```

### 4.2 Configuração por Arquivo
```bash
# Arquivo de configuração: ~/.wavemonrc ou /etc/wavemon.conf
# Exemplo de configuração
cat > ~/.wavemonrc << 'EOF'
# Configurações do Wavemon
interface = wlan0
refresh_interval = 2.0
histogram_samples = 60
show_histogram = yes
show_quality = yes
use_si_units = no
color_signal = green
color_noise = red
color_quality = cyan
auto_interface = no
monitor_detection = yes
log_file = /var/log/wavemon.log
log_enable = no
EOF

# Carregar configuração
wavemon --config ~/.wavemonrc
```

### 4.3 Modo Script/Non-interactive
```bash
#!/bin/bash
# Exemplo: Monitorar e alertar quando sinal cai abaixo do limite

INTERFACE="wlan0"
THRESHOLD="-70"  # dBm
CHECK_INTERVAL=30

while true; do
    # Capturar nível de sinal
    SIGNAL=$(wavemon -i "$INTERFACE" -q -c 1 | grep "Signal Level" | awk '{print $3}')
    
    # Remover o "dBm" e converter para número
    SIGNAL_VALUE=${SIGNAL%dBm}
    
    # Comparar com threshold
    if [ "$SIGNAL_VALUE" -lt "$THRESHOLD" ]; then
        echo "ALERTA: Sinal fraco detectado: $SIGNAL"
        notify-send "Wi-Fi Alert" "Sinal fraco: $SIGNAL"
        # Ações adicionais podem ser adicionadas aqui
    fi
    
    sleep "$CHECK_INTERVAL"
done
```

### 4.4 Integração com Outras Ferramentas
```bash
# Combinar com iw para mais informações
iw dev wlan0 station dump | grep -E "(signal|tx bitrate|rx bitrate)"
wavemon -i wlan0 -q | grep "Signal Level"

# Usar com watch para monitoramento contínuo
watch -n 2 'wavemon -i wlan0 -q -c 1'

# Integrar com sistemas de monitoramento (Prometheus)
#!/bin/bash
# export-wifi-stats.sh
INTERFACE="wlan0"
METRICS_FILE="/var/lib/node_exporter/wifi.prom"

while true; do
    STATS=$(wavemon -i "$INTERFACE" -q -c 1)
    
    SIGNAL=$(echo "$STATS" | grep "Signal Level" | awk '{print $3}' | sed 's/dBm//')
    NOISE=$(echo "$STATS" | grep "Noise Level" | awk '{print $3}' | sed 's/dBm//')
    QUALITY=$(echo "$STATS" | grep "Quality:" | awk '{print $2}' | sed 's|/.*||')
    
    cat > "$METRICS_FILE" << EOF
# HELP wifi_signal Signal strength in dBm
# TYPE wifi_signal gauge
wifi_signal{interface="$INTERFACE"} $SIGNAL
# HELP wifi_noise Noise level in dBm
# TYPE wifi_noise gauge
wifi_noise{interface="$INTERFACE"} $NOISE
# HELP wifi_quality Connection quality (0-100)
# TYPE wifi_quality gauge
wifi_quality{interface="$INTERFACE"} $QUALITY
EOF
    
    sleep 10
done
```

## 🔍 Capítulo 5: Análise e Interpretação

### 5.1 Interpretação dos Valores de Sinal

#### Tabela de Referência dBm:
```
dBm     | Qualidade       | Descrição
--------|-----------------|----------------------------------------
-30     | Excelente       | Sinal muito forte (próximo ao AP)
-50     | Muito Bom       | Conexão excelente
-60     | Bom             | Conexão boa para maioria dos usos
-67     | Satisfatório    | Mínimo para streaming HD
-70     | Fraco           | Conexão básica, pode ter instabilidade
-80     | Muito Fraco     | Conexão muito lenta/intermitente
-90     | Sem conexão     | Limite prático de detecção
```

#### Cálculo de SNR (Signal-to-Noise Ratio):
```bash
# SNR = Sinal - Ruído
# Exemplo: Sinal: -55 dBm, Ruído: -92 dBm
# SNR = (-55) - (-92) = 37 dB

# Interpretação do SNR:
# > 40 dB: Excelente
# 25-40 dB: Muito Bom
# 15-25 dB: Bom
# 10-15 dB: Aceitável
# < 10 dB: Ruim
```

### 5.2 Análise de Problemas Comuns

#### Problema: Sinal Flutuante
```
Sintomas:
- Histograma mostra variações bruscas
- Quality oscila rapidamente
- Bitrate muda frequentemente

Possíveis causas:
- Interferência de outros dispositivos
- Múltiplos APs no mesmo canal
- Obstáculos físicos em movimento

Diagnóstico no Wavemon:
1. Verificar histograma na Level View
2. Analisar canal na Info View (interferência)
3. Verificar modo e largura do canal
```

#### Problema: Conexão Lenta
```
Sintomas:
- Signal bom mas throughput baixo
- Muitos erros/drops na Info View
- Bitrate abaixo do esperado

Diagnóstico:
1. Info View: Verificar erros e drops
2. Level View: Comparar Signal vs Noise
3. Scan View: Verificar congestionamento de canais

Comandos de diagnóstico:
$ iw dev wlan0 station dump  # Estatísticas detalhadas
$ iw dev wlan0 link          # Status do link
```

### 5.3 Otimização de Canal Wi-Fi
```bash
# Usar Wavemon para escolher melhor canal
#!/bin/bash
# find-best-channel.sh
INTERFACE="wlan0"

echo "Analisando redes próximas..."
wavemon -i "$INTERFACE" -s

# Após scan, analisar manualmente:
# 1. Identificar canais menos congestionados
# 2. Preferir canais 1, 6, 11 (2.4GHz sem sobreposição)
# 3. Para 5GHz, escolher canais DFS se disponível

# Configurar canal manualmente
sudo iw dev "$INTERFACE" set channel 6
# Ou via hostapd/configuração do roteador
```

### 5.4 Monitoramento de Segurança
```bash
# Detectar redes suspeitas
#!/bin/bash
# monitor-rogue-ap.sh
INTERFACE="wlan0"
KNOWN_NETWORKS="/etc/known-networks.txt"

while true; do
    # Scan e filtrar redes desconhecidas
    wavemon -i "$INTERFACE" -s -q | grep -v -f "$KNOWN_NETWORKS" > /tmp/unknown.txt
    
    if [ -s /tmp/unknown.txt ]; then
        echo "ALERTA: Redes desconhecidas detectadas:"
        cat /tmp/unknown.txt
        # Enviar alerta por email/notificação
    fi
    
    sleep 300  # Verificar a cada 5 minutos
done
```

## 🛠️ Capítulo 6: Troubleshooting

### 6.1 Problemas Comuns e Soluções

#### Problema: "Interface not found or not wireless"
```bash
# Verificar interfaces disponíveis
ip link show

# Verificar se interface é wireless
iw dev

# Soluções:
# 1. Especificar interface corretamente
sudo wavemon -i wlan0

# 2. Verificar se driver está carregado
lsmod | grep -i wifi

# 3. Ativar interface
sudo ip link set wlan0 up

# 4. Verificar permissões (executar como root)
sudo wavemon
```

#### Problema: "No scan results" na Scan View
```bash
# Causas possíveis:
# 1. Interface em modo monitor
sudo iw dev wlan0 set type managed

# 2. Interface não está up
sudo ip link set wlan0 up

# 3. Região/configuração restritiva
sudo iw reg set BO   # Bolívia - menos restrições
# OU
country=US
sudo iw reg set "$country"

# 4. Problemas de driver
sudo modprobe -r nome_do_driver
sudo modprobe nome_do_driver
```

#### Problema: Valores de sinal incorretos/zero
```bash
# Verificar compatibilidade do driver
iw list | grep -A5 "Supported interface modes"

# Testar com iwconfig para comparação
iwconfig wlan0

# Verificar se nl80211 está sendo usado
iw dev wlan0 info | grep -i "wiphy"

# Alternar para wireless-tools (legado)
# Editar configuração do wavemon para usar wext
```

### 6.2 Debug Detalhado
```bash
# Executar wavemon com debug máximo
sudo WAVEMON_DEBUG=3 wavemon -i wlan0

# Verificar logs do sistema
sudo dmesg | tail -20
journalctl -f | grep -i wifi

# Testar interface com ferramentas nativas
sudo iw dev wlan0 scan | head -20
iwconfig wlan0
iw dev wlan0 link

# Verificar erros específicos
strace -o wavemon.log wavemon -i wlan0
```

### 6.3 Problemas Específicos por Driver

#### Intel WiFi (iwlwifi):
```bash
# Habilitar debug
sudo modprobe iwlwifi debug=0x1

# Verificar firmware
sudo dmesg | grep -i firmware

# Opções do módulo (criar /etc/modprobe.d/iwlwifi.conf):
options iwlwifi power_save=0
options iwlwifi 11n_disable=1  # Desabilitar 802.11n se instável
```

#### Realtek (rtl8xxxu, rtlwifi):
```bash
# Drivers frequentemente problemáticos
# Tentar driver alternativo
sudo modprobe -r rtl8xxxu
sudo modprobe rtl8192cu

# Ou usar driver de backports
# https://github.com/lwfinger/rtlwifi_new
```

#### Atheros (ath9k, ath10k):
```bash
# Geralmente mais estáveis
# Verificar opções específicas
sudo modprobe ath9k nohwcrypt=1
```

### 6.4 Problemas de Permissão
```bash
# Solução 1: Executar como root (simples)
sudo wavemon

# Solução 2: Adicionar usuário ao grupo netdev
sudo usermod -aG netdev $USER

# Solução 3: Configurar capabilities
sudo setcap cap_net_admin+ep /usr/bin/wavemon

# Solução 4: Configurar sudo sem senha
echo "$USER ALL=(ALL) NOPASSWD: /usr/bin/wavemon" | sudo tee /etc/sudoers.d/wavemon
```

## 📈 Capítulo 7: Casos de Uso Avançados

### 7.1 Monitoramento de Múltiplas Interfaces
```bash
#!/bin/bash
# multi-interface-monitor.sh
INTERFACES=("wlan0" "wlan1" "wlp2s0")

for IFACE in "${INTERFACES[@]}"; do
    if ip link show "$IFACE" &>/dev/null; then
        echo "=== Monitorando $IFACE ==="
        gnome-terminal --title="Wavemon $IFACE" -- sudo wavemon -i "$IFACE"
    fi
done

# Monitorar alternadamente
while true; do
    for IFACE in "${INTERFACES[@]}"; do
        clear
        echo "=== $IFACE ==="
        sudo wavemon -i "$IFACE" -q -c 1
        sleep 5
    done
done
```

### 7.2 Análise de Cobertura Wi-Fi
```bash
#!/bin/bash
# wifi-coverage-map.sh
LOG_FILE="coverage-$(date +%Y%m%d).csv"
INTERFACE="wlan0"

echo "Timestamp,Location,Signal(dBm),Noise(dBm),Quality,Bitrate(Mbps)" > "$LOG_FILE"

echo "Inicie o mapeamento. Pressione Enter a cada localização."
echo "Pressione Ctrl+C para terminar."

LOCATION=1
while true; do
    read -p "Localização $LOCATION (ou 'q' para sair): " input
    
    if [[ "$input" == "q" ]]; then
        break
    fi
    
    # Capturar métricas
    STATS=$(sudo wavemon -i "$INTERFACE" -q -c 1)
    SIGNAL=$(echo "$STATS" | grep "Signal Level" | awk '{print $3}')
    NOISE=$(echo "$STATS" | grep "Noise Level" | awk '{print $3}')
    QUALITY=$(echo "$STATS" | grep "Quality:" | awk '{print $2}' | cut -d'/' -f1)
    BITRATE=$(echo "$STATS" | grep "Bitrate:" | awk '{print $2}')
    
    # Registrar
    TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')
    echo "$TIMESTAMP,Loc$LOCATION,$SIGNAL,$NOISE,$QUALITY,$BITRATE" >> "$LOG_FILE"
    
    echo "Registrado: Sinal $SIGNAL, Qualidade $QUALITY/100"
    ((LOCATION++))
done

echo "Dados salvos em $LOG_FILE"
```

### 7.3 Detecção de Ataques/Interferências
```bash
#!/bin/bash
# wifi-intrusion-detection.sh
INTERFACE="wlan0"
BASELINE_FILE="/tmp/wifi-baseline.txt"
ALERT_THRESHOLD=10  # dB de mudança significativa

# Estabelecer baseline
echo "Estabelecendo baseline por 30 segundos..."
sudo wavemon -i "$INTERFACE" -q -c 30 | grep "Signal Level" | awk '{print $3}' | sed 's/dBm//' > "$BASELINE_FILE"

BASELINE_AVG=$(awk '{sum+=$1} END {print sum/NR}' "$BASELINE_FILE")
echo "Baseline estabelecida: $BASELINE_AVG dBm"

# Monitorar desvios
while true; do
    CURRENT_SIGNAL=$(sudo wavemon -i "$INTERFACE" -q -c 1 | grep "Signal Level" | awk '{print $3}' | sed 's/dBm//')
    
    DEVIATION=$(echo "$BASELINE_AVG - $CURRENT_SIGNAL" | bc)
    
    if (( $(echo "$DEVIATION > $ALERT_THRESHOLD" | bc -l) )); then
        echo "ALERTA: Desvio significativo detectado!"
        echo "Baseline: $BASELINE_AVG dBm, Atual: $CURRENT_SIGNAL dBm"
        echo "Desvio: $DEVIATION dB"
        # Executar scan rápido
        sudo wavemon -i "$INTERFACE" -s -q | head -20
    fi
    
    sleep 5
done
```

### 7.4 Integração com Grafana/Prometheus
```yaml
# docker-compose.yml para stack de monitoramento
version: '3'
services:
  wavemon-exporter:
    build: .
    ports:
      - "9111:9111"
    volumes:
      - /proc/net/dev:/proc/net/dev:ro
      - /sys/class/net:/sys/class/net:ro
    cap_add:
      - NET_ADMIN
    network_mode: "host"

# Exporter em Python
cat > wavemon_exporter.py << 'EOF'
#!/usr/bin/env python3
from prometheus_client import start_http_server, Gauge
import subprocess
import time

# Métricas
signal_level = Gauge('wifi_signal_dbm', 'WiFi signal strength in dBm', ['interface'])
noise_level = Gauge('wifi_noise_dbm', 'WiFi noise level in dBm', ['interface'])
quality = Gauge('wifi_quality', 'WiFi connection quality (0-100)', ['interface'])
bitrate = Gauge('wifi_bitrate_mbps', 'WiFi connection bitrate in Mbps', ['interface'])

def collect_metrics(interface='wlan0'):
    try:
        # Executar wavemon e capturar saída
        cmd = ['sudo', 'wavemon', '-i', interface, '-q', '-c', '1']
        result = subprocess.run(cmd, capture_output=True, text=True, timeout=5)
        
        # Parsear saída
        for line in result.stdout.split('\n'):
            if 'Signal Level' in line:
                value = line.split()[2].replace('dBm', '')
                signal_level.labels(interface=interface).set(float(value))
            elif 'Noise Level' in line:
                value = line.split()[2].replace('dBm', '')
                noise_level.labels(interface=interface).set(float(value))
            elif 'Quality:' in line:
                value = line.split()[1].split('/')[0]
                quality.labels(interface=interface).set(float(value))
            elif 'Bitrate:' in line:
                value = line.split()[1].replace('Mb/s', '')
                bitrate.labels(interface=interface).set(float(value))
                
    except Exception as e:
        print(f"Erro coletando métricas: {e}")

if __name__ == '__main__':
    start_http_server(9111)
    print("Exportador iniciado na porta 9111")
    
    while True:
        collect_metrics('wlan0')
        time.sleep(10)
EOF
```

## 📚 Capítulo 8: Referência Rápida

### 8.1 Teclas de Atalho
```
Geral:
  q, Ctrl+C      - Sair
  h, F5          - Ajuda
  ?              - Atalhos de teclado

Navegação:
  F1             - Level View (Monitoramento)
  F2             - Info View (Informações)
  F3             - Scan View (Scan de redes)
  F4             - Prefs View (Preferências)
  Tab            - Alternar entre elementos
  Setas          - Navegar em listas/menus
  Enter          - Selecionar/Confirmar

Level View:
  r              - Resetar estatísticas
  c              - Limpar histograma
  +              - Aumentar intervalo de atualização
  -              - Diminuir intervalo de atualização

Scan View:
  S              - Iniciar scan
  C              - Conectar à rede selecionada
  R              - Atualizar lista
  Espaço         - Selecionar rede
  /              - Buscar rede
  F              - Filtrar por segurança
```

### 8.2 Interpretação de Códigos de Segurança
```
WEP        - Wireless Encryption Protocol (inseguro)
WPA        - Wi-Fi Protected Access
WPA2       - WPA2 (recomendado)
WPA3       - WPA3 (mais recente e seguro)
Open       - Sem segurança (inseguro)
802.1X     - Enterprise authentication
WPA/WPA2   - Modo misto (compatibilidade)
```

### 8.3 Modos de Operação Wireless
```
Managed    - Cliente normal (conecta a AP)
Master     - Ponto de acesso
Ad-hoc     - Rede ponto-a-ponto
Monitor    - Modo monitor (captura pacotes)
Mesh       - Rede mesh
```

### 8.4 Comandos Equivalentes
```
Wavemon           | Comando nativo
------------------|----------------------
Level View        | iwconfig, iw dev link
Info View         | iw list, iw dev info
Scan View         | iw dev scan, iwlist scan
Signal monitoring | watch -n1 iwconfig
Statistics        | cat /proc/net/wireless
```

## 🔮 Capítulo 9: Dicas e Melhores Práticas

### 9.1 Para Administradores de Rede
```bash
# 1. Monitoramento proativo
# Criar script de monitoramento contínuo
cat > /usr/local/bin/wifi-monitor << 'EOF'
#!/bin/bash
INTERFACE=${1:-wlan0}
LOG_DIR="/var/log/wifi-monitor"
mkdir -p "$LOG_DIR"

while true; do
    TIMESTAMP=$(date +%Y%m%d-%H%M)
    sudo wavemon -i "$INTERFACE" -q -c 60 > "$LOG_DIR/wifi-$TIMESTAMP.log"
    sleep 1
done
EOF

# 2. Alertas automáticos
# Configurar monitoramento de threshold
sudo apt install mailutils
# Adicionar alertas por email quando sinal < -75dBm

# 3. Relatórios diários
# Gerar relatório de qualidade de conexão
0 2 * * * root /usr/local/bin/generate-wifi-report.sh
```

### 9.2 Para Usuários Domésticos
```bash
# 1. Encontrar melhor localização para roteador
#!/bin/bash
# test-router-placement.sh
echo "Teste diferentes locais para o roteador:"
echo "1. Coloque o dispositivo no local desejado"
echo "2. Execute este script"
echo "3. Anote os resultados"

read -p "Nome do local: " location
sudo wavemon -i wlan0 -q -c 10 | tee "test-$location.txt"
echo "Resultados salvos em test-$location.txt"

# 2. Verificar interferência de dispositivos
# Desligue dispositivos Bluetooth, micro-ondas, etc.
# Compare os níveis de sinal e ruído
```

### 9.3 Para Desenvolvedores
```bash
# 1. Integrar com aplicações Python
import subprocess
import json

def get_wifi_stats(interface='wlan0'):
    """Obtém estatísticas WiFi via wavemon"""
    cmd = ['sudo', 'wavemon', '-i', interface, '-q', '-c', '1']
    result = subprocess.run(cmd, capture_output=True, text=True)
    
    stats = {}
    for line in result.stdout.split('\n'):
        if ':' in line:
            key, value = line.split(':', 1)
            stats[key.strip()] = value.strip()
    
    return stats

# 2. Criar API REST
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/api/wifi/stats')
def wifi_stats():
    return jsonify(get_wifi_stats())

if __name__ == '__main__':
    app.run(port=5000)
```

## 🎓 Capítulo 10: Recursos de Aprendizado

### 10.1 Documentação Oficial
```bash
# Manual pages
man wavemon

# Ajuda integrada
wavemon -h

# Página do projeto
# https://github.com/uoaerg/wavemon

# Wiki e exemplos
# https://github.com/uoaerg/wavemon/wiki
```

### 10.2 Comunidade e Suporte
```
Fóruns:
- https://github.com/uoaerg/wavemon/issues
- https://forums.linuxmint.com/
- https://forum.manjaro.org/

IRC:
- #wireless no Freenode (agora Libera Chat)

Listas de Email:
- linux-wireless@vger.kernel.org
```

### 10.3 Ferramentas Relacionadas
```bash
# Complementares ao Wavemon
sudo apt install:
  iw          # Ferramentas wireless modernas
  wireless-tools  # iwconfig, iwlist
  wavemon     # Este manual
  aircrack-ng # Testes de segurança
  kismet      # Detector wireless
  wifite      # Auditoria automatizada

# GUI alternatives
  linssid     # Scanner gráfico
  wicd        # Gerenciador de conexão
  nm-applet   # NetworkManager
```

## 📊 Apêndice: Exemplos Práticos

### A.1 Script de Monitoramento Completo
```bash
#!/bin/bash
# wifi-health-check.sh
# Script completo de diagnóstico de WiFi

INTERFACE=${1:-wlan0}
REPORT_FILE="wifi-health-$(date +%Y%m%d-%H%M%S).txt"

echo "=== WiFi Health Check Report ===" > "$REPORT_FILE"
echo "Data: $(date)" >> "$REPORT_FILE"
echo "Interface: $INTERFACE" >> "$REPORT_FILE"
echo "=================================" >> "$REPORT_FILE"

# 1. Informações básicas
echo -e "\n1. INFORMAÇÕES DA INTERFACE:" >> "$REPORT_FILE"
iw dev "$INTERFACE" info >> "$REPORT_FILE" 2>&1

# 2. Status atual
echo -e "\n2. STATUS DA CONEXÃO:" >> "$REPORT_FILE"
wavemon -i "$INTERFACE" -q -c 3 >> "$REPORT_FILE" 2>&1

# 3. Scan de redes
echo -e "\n3. REDES DISPONÍVEIS:" >> "$REPORT_FILE"
timeout 10 sudo wavemon -i "$INTERFACE" -s -q >> "$REPORT_FILE" 2>&1

# 4. Análise de canal
echo -e "\n4. ANÁLISE DE CANAL:" >> "$REPORT_FILE"
sudo iw dev "$INTERFACE" scan | grep -E "(freq:|signal:|SSID:)" | head -30 >> "$REPORT_FILE" 2>&1

# 5. Recomendações
echo -e "\n5. RECOMENDAÇÕES:" >> "$REPORT_FILE"
echo "Baseado na análise:" >> "$REPORT_FILE"

# Analisar sinal
SIGNAL=$(grep "Signal Level" "$REPORT_FILE" | tail -1 | awk '{print $3}' | sed 's/dBm//')
if [ "$SIGNAL" -gt -50 ]; then
    echo "- ✅ Sinal excelente ($SIGNAL dBm)" >> "$REPORT_FILE"
elif [ "$SIGNAL" -gt -65 ]; then
    echo "- 👍 Sinal bom ($SIGNAL dBm)" >> "$REPORT_FILE"
elif [ "$SIGNAL" -gt -75 ]; then
    echo "- ⚠️  Sinal fraco ($SIGNAL dBm)" >> "$REPORT_FILE"
    echo "  Considere: Mover roteador, usar repetidor" >> "$REPORT_FILE"
else
    echo "- ❌ Sinal muito fraco ($SIGNAL dBm)" >> "$REPORT_FILE"
    echo "  Ação necessária: Reposicionar equipamentos" >> "$REPORT_FILE"
fi

echo -e "\nRelatório salvo em: $REPORT_FILE"
echo "Use: cat $REPORT_FILE para ver resultados"
```

### A.2 Dashboard Terminal com TMUX
```bash
#!/bin/bash
# wifi-dashboard.sh
# Dashboard com múltiplas visualizações usando TMUX

SESSION="wifi-dashboard"
INTERFACE="wlan0"

# Criar nova sessão TMUX
tmux new-session -d -s "$SESSION"

# Dividir painel
tmux split-window -h
tmux split-window -v

# Painel 1: Wavemon Level View
tmux select-pane -t 0
tmux send-keys "sudo wavemon -i $INTERFACE -l" C-m

# Painel 2: Scan contínuo
tmux select-pane -t 1
tmux send-keys "watch -n 10 'sudo wavemon -i $INTERFACE -s -q | head -20'" C-m

# Painel 3: Estatísticas do sistema
tmux select-pane -t 2
tmux send-keys "watch -n 2 'echo \"=== ifconfig ===\"; ifconfig $INTERFACE | grep -E \"(RX|TX|packets)\"; echo; echo \"=== Wireless ===\"; iwconfig $INTERFACE | grep -E \"(Signal|Bit|ESSID)\"; echo; echo \"=== Conectados ===\"; iw dev $INTERFACE station dump | grep -c \"Station\" | xargs echo \"Dispositivos conectados: \"'" C-m

# Anexar à sessão
tmux attach-session -t "$SESSION"
```

---

## 🏁 Conclusão

### Resumo das Vantagens do Wavemon
- ✅ **Interface intuitiva**: TUI bem organizada e fácil de usar
- ✅ **Monitoramento em tempo real**: Atualização contínua de métricas
- ✅ **Informações detalhadas**: Muito mais que apenas nível de sinal
- ✅ **Multiplataforma**: Funciona na maioria das distribuições Linux
- ✅ **Leve e eficiente**: Consumo mínimo de recursos

### Fluxo de Trabalho Recomendado
1. **Diagnóstico inicial**: Level View para ver sinal/ruído
2. **Análise detalhada**: Info View para estatísticas
3. **Otimização**: Scan View para escolher melhor canal
4. **Configuração**: Prefs View para ajustes pessoais
5. **Monitoramento contínuo**: Scripts automatizados

### Próximos Passos Sugeridos
1. **Automatize** o monitoramento com scripts
2. **Integre** com sistemas de alerta (Email, Telegram)
3. **Crie dashboards** com Grafana para visualização histórica
4. **Contribua** com o projeto no GitHub
5. **Experimente** diferentes drivers e configurações

### Dica Final
> *"O Wavemon não é apenas uma ferramenta de diagnóstico, mas um professor que ajuda a entender o comportamento complexo das redes wireless. Use-o não apenas quando tiver problemas, mas para aprender e otimizar proativamente sua rede."*

---

## 📋 Checklist de Monitoramento WiFi

- [ ] Nível de sinal acima de -65 dBm
- [ ] SNR (Signal-to-Noise Ratio) acima de 25 dB
- [ ] Taxa de erro abaixo de 1%
- [ ] Canal sem interferência significativa
- [ ] Largura de canal apropriada para ambiente
- [ ] Segurança WPA2 ou WPA3 habilitada
- [ ] Driver atualizado e estável
- [ ] Firmware atualizado (se aplicável)
- [ ] Sem sobreaquecimento do dispositivo
- [ ] Posicionamento otimizado dos equipamentos

**Bom monitoramento!** 📶

---
*Manual atualizado para Wavemon v0.9.4*  
*Última revisão: [Data]*  
*Contribuições são bem-vindas via GitHub Issues*
