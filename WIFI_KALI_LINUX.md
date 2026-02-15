
# 📘 Manual Completo: Como Fazer o Kali Linux Reconhecer uma Placa de Rede Externa no VirtualBox ou VMware

## 1. Introdução

Um dos grandes poderes do Kali Linux em uma máquina virtual (VM) é a possibilidade de utilizar hardware externo, como placas de rede USB, para realizar testes de penetração (pentests) sem a necessidade de um dual boot. No entanto, por questões de segurança e isolamento, as máquinas virtuais não têm acesso direto ao hardware do computador anfitrião (host) por padrão.

Este manual tem como objetivo guiá-lo, passo a passo, no processo de "passar" o controle de uma placa de rede USB do seu sistema anfitrião (Windows, Linux ou macOS) para o Kali Linux rodando no VirtualBox ou VMware, garantindo que ele seja reconhecido como uma interface de rede (ex: `wlan0` ou `wlan1`).

---

## 2. Pré-requisitos

Antes de começar, certifique-se de que possui os seguintes itens:

-   **Kali Linux instalado** em uma máquina virtual (VirtualBox ou VMware).
-   **Placa de Rede USB Compatível**: Preferencialmente uma que seja bem suportada no Linux, como as que utilizam chipsets **Atheros (ar9271)** ou **Realtek (rtl88xxau)**. Muitas placas usadas para pentest (como as da Alfa Network) são ideais.
-   **Acesso à Internet** no anfitrião para possíveis downloads de drivers e pacotes.

---

## 3. Preparação no Sistema Anfitrião (Host)

O primeiro passo é garantir que o seu computador anfitrião reconhece a placa. Se o host não a vir, a VM também não a verá.

1.  **Conecte a Placa USB**: Insira a placa de rede em uma porta USB do seu computador.
2.  **Verificação no Host**:
    -   **No Windows**: Abra o **Gerenciador de Dispositivos**. A placa deve aparecer em "Adaptadores de Rede" ou "Controladores Universal Serial Bus". Se aparecer com um ponto de exclamação amarelo, o driver do Windows pode não estar instalado corretamente, mas isso não impede o funcionamento no Linux, desde que o dispositivo seja detectado como hardware USB .
    -   **No Linux**: Abra um terminal e digite `lsusb`. Você deverá ver o fabricante da sua placa na lista (ex: `Atheros Communications`, `Realtek Semiconductor Corp.`).

---

## 4. Configuração no VirtualBox

O VirtualBox requer a instalação de um pacote adicional para suporte USB avançado.

### 4.1. Instalar o Pacote de Extensão (Extension Pack)
O suporte a USB 2.0 e 3.0 no VirtualBox não é nativo na versão base. É necessário instalar o **Oracle VM VirtualBox Extension Pack**.

1.  Acesse o site oficial do VirtualBox e baixe a versão do Extension Pack correspondente à sua versão do VirtualBox.
2.  Abra o VirtualBox, vá em **Arquivo (File) > Preferências (Preferences) > Extensões (Extensions)**.
3.  Clique no ícone ao lado direito e adicione o arquivo baixado.

### 4.2. Configurar a Máquina Virtual
1.  Selecione a máquina virtual do Kali e clique em **Configurações (Settings)**.
2.  Vá até a aba **USB**.
3.  Marque a opção **"Habilitar Controlador USB" (Enable USB Controller)**.
4.  Selecione o controlador correto:
    -   Se sua placa for USB 3.0, escolha **"USB 3.0 (xHCI) Controller"**. (É a opção mais comum para placas modernas) .
    -   Se for USB 2.0, escolha "USB 2.0 (EHCI) Controller".
5.  Na seção **"Filtros de Dispositivos USB" (USB Device Filters)** , clique no ícone de **"Adicionar"** (o símbolo de "+" com um cabo USB). Selecione sua placa de rede na lista que aparecer. Isso fará com que, ao iniciar a VM, ela "capture" automaticamente a placa .

### 4.3. Considerações Importantes para o VirtualBox
-   **Grupo de Usuários (Linux Hosts)**: Se o seu sistema anfitrião for Linux, o usuário precisa pertencer ao grupo `vboxusers` para ter permissão de acessar dispositivos USB. Para adicionar, use: `sudo usermod -aG vboxusers $USER` (substitua `$USER` pelo seu nome de usuário) e reinicie a sessão .
-   **Drivers Antigos no Windows**: Se após os passos a placa não aparecer, pode haver conflito com drivers antigos do VirtualBox no Windows. É recomendado, em casos extremos, desinstalar o VirtualBox e apagar manualmente os arquivos `vbox.sys` da pasta `C:\Windows\System32\drivers` antes de reinstalar a versão mais recente .

---

## 5. Configuração no VMware (Workstation/Player)

O processo no VMware é geralmente mais direto, pois o suporte a USB costuma vir integrado.

### 5.1. Configurar a Máquina Virtual
1.  Com a máquina virtual desligada, vá em **Editar configurações da máquina virtual (Edit virtual machine settings)**.
2.  Certifique-se de que o **Controlador USB** está presente e configurado para compatibilidade com USB 3.0 .
3.  **Inicie a Kali Linux**.

### 5.2. Conectar o Dispositivo
1.  Com a VM já rodando, vá ao menu da VM: **Máquina Virtual (VM) > Dispositivos Removíveis (Removable Devices)**.
2.  Procure pelo nome da sua placa de rede USB na lista.
3.  Passe o mouse sobre ela e clique em **"Conectar (desconectar do host)" (Connect (Disconnect from host))** .

---

## 6. Verificação e Configuração no Kali Linux (Guest)

Após conectar a placa à VM, é hora de verificar se o Kali a reconhece.

1.  **Abra um terminal** no Kali (`Ctrl + Alt + T`).
2.  **Verifique a detecção USB**:
    ```bash
    lsusb
    ```
    Você deve ver o fabricante da sua placa listado. Exemplo: `Realtek Semiconductor Corp. RTL8812AU 802.11a/b/g/n/ac Wireless Adapter` .
3.  **Verifique as interfaces de rede**:
    ```bash
    ip a
    ```
    ou
    ```bash
    ifconfig
    ```
    Procure por uma nova interface. O nome mais comum é `wlan0`, mas devido a políticas de nomenclatura previsível, pode aparecer como `wlx<span>...</span>` (um nome baseado no endereço MAC) .
4.  **Verifique o modo wireless**:
    ```bash
    iwconfig
    ```
    Se a interface aparecer aqui, significa que o kernel a reconheceu como uma interface de rede sem fio.

---

## 7. Resolução de Problemas (Troubleshooting)

Aqui estão os problemas mais comuns e como resolvê-los:

### 7.1. A Placa Aparece no `lsusb`, mas não no `ip a` ou `iwconfig`

**Problema**: O Linux detectou o hardware USB, mas não carregou o driver correto para transformá-lo em uma interface de rede.

**Solução**: Instalar o driver/firmware manualmente.

-   **Para placas Realtek**: Muitas placas de pentest usam chipsets como `rtl88xxau`. O Kali pode não vir com eles instalados.
    ```bash
    sudo apt update
    sudo apt install realtek-rtl88xxau-dkms
    ```
    (Este pacote compila o driver para o seu kernel atual) .
-   **Para placas específicas (ex: Alfa AWUS1900 - chipset rtl8814au)** :
    Às vezes, o driver precisa ser compilado diretamente da fonte, como no caso da Alfa AWUS1900 (chipset rtl8814au) :
    ```bash
    sudo apt update
    sudo apt install dkms build-essential libelf-dev linux-headers-$(uname -r) git
    git clone https://github.com/aircrack-ng/rtl8814au.git
    cd rtl8814au
    sudo make dkms_install
    ```
    Após a instalação, reinicie a VM ou recarregue o módulo.

-   **Para placas Atheros**: Instale os firmwares:
    ```bash
    sudo apt install firmware-atheros
    ```

Após instalar, reinicie a VM ou tente carregar o módulo manualmente (ex: `sudo modprobe rtl88xxau`).

### 7.2. Dispositivo Funciona Apenas Após Reiniciar a VM

**Problema**: Bug conhecido no VirtualBox onde a inicialização do firmware falha na primeira tentativa, mas funciona no reboot .

**Solução**: Após conectar a placa e ela não funcionar, simplesmente reinicie a máquina virtual (`reboot` no terminal) com a placa ainda conectada. Após o reboot, o dispositivo geralmente é inicializado corretamente .

### 7.3. "Erro: Failed to attach the USB device"

**Problema**: O VirtualBox não conseguiu "tomar" o controle do dispositivo do host.

**Solução**:
-   Certifique-se de que nenhum programa no host está usando o dispositivo.
-   No Windows, evite "ejetar" o dispositivo. Apenas desconecte-o da VM pelo menu.
-   No Linux host, verifique as permissões do grupo `vboxusers` mencionado anteriormente.

### 7.4. A Interface Aparece, mas Não Consegue Fazer Ataques (ex: "Operation not permitted")

**Problema**: Embora o driver esteja carregado, a interface pode não estar em modo monitor ou pode haver restrições.

**Solução**:
-   Coloque a interface em modo monitor (requer que o driver suporte isso):
    ```bash
    sudo ip link set wlan0 down
    sudo iw dev wlan0 set type monitor
    sudo ip link set wlan0 up
    ```
-   Verifique com `iwconfig`. Deve aparecer "Mode:Monitor".

