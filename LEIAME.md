
# Meu Guia de Instalação do Arch Linux com Hyprland
> Este repositório contém **MEU guia personalizado de instalação e configuração desta distribuição. O foco desta configuração é o minimalismo e a performance**, instalando apenas os pacotes essenciais **para um ambiente de trabalho fluido**, evitando o bloatware (pacotes desnecessários) de instalações padrão.

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

<i>Esses são os passos que seguiremos:</i></h4>
<details>
    <summary><b>1. Preparação </b></summary>

Nessa parte, vamos focar em preparar o ambiente para **baixar a ISO**, **preparar o pendrive bootável**, **baixar o Ventoy** (o programa que será **usado para criar o pendrive bootável**) e depois **criar o pendrive bootável**.
</details>
<details>
    <summary><b>2. Configuração da BIOS </b></summary>

**Apenas colocaremos o pendrive pra iniciar primeiro que qualquer sistema.**
</details>
<details>
    <summary><b>3. Configuração básica do ambiente Live </b></summary>

Nesta etapa faremos algumas **configurações básicas no ambiente Live**. Essas configurações serão para **configurar o layout do teclado, internet, sincronizar o relógio e a correção do chaveiro.**
Nesse passo, faremos somente algumas **configurações básicas no ambiente Live da ISO**. Essas configurações serão pra **configurar o layout do teclado, configurar a internet, sincronizar o relógio e corrigir possiveis erros de `keyring`.**
</details>
<details>
    <summary><b>4. Particionamento do disco </b></summary>

Agora, o foco será **organizar o disco** (HD ou SSD). Vamos **decidir como o espaço será dividido, definindo a partição de `boot` para o sistema iniciar, a partição de `swap`** (memória auxiliar) para ajudar o computador quando ele estiver sobrecarregado, **e a partição `root`, que é a partição principal** onde todos os arquivos e o sistema operacional serão armazenados.
</details>
<details>
    <summary><b>5. Instalação da base </b></summary>

Aqui, **atualizaremos os espelhos e também usaremos `pacstrap` para instalar o Kernel Linux** e alguns pacotes essenciais. Por fim, vamos **gerar o arquivo `fstab` com o comando `genfstab` para lembrar e montar automaticamente as partições** que você havia montado anteriormente manualmente na etapa 4.
</details>
<details>
    <summary><b>6. Configurações iniciais do sistema </b></summary>

Após a instalação, vamos nos concentrar na **configuração interna do sistema. Pra isso precisaremos ativar uma biblioteca importante chamada `multilib`, vamos configurar o fuso horário, o local, o idioma, o nome do computador (`hostname`). Baixaremos e configuraremos o bootloader** (GRUB) para que ele inicie corretamente e, finalmente, **criaremos o usuário com privilégios de administrador**.
</details> 
<details>
    <summary><b>7. Activate Services and Finish the Installation </b></summary>

Por fim, **ativaremos dois serviços para que o sistema inicie corretamente** e **desmontaremos o ambiente Live e reiniciaremos a máquina.**
</details> 

---

# 📢┃Aviso Importante
> **Esse tutorial foi <strong><u>projetado e desenvolvido para ser fácil de entender e de executar</u></strong>. No entanto, para garantir uma instalação bem-sucedida e evitar falhas no sistema, <strong><u>é essencial que todas as instruções sejam rigorosamente seguidas</strong></u>. Certifique-se de digitar os comandos exatamente como descritos, respeitando os espaços, as letras maiúsculas e os caracteres especiais.**

---
# 🛠️┃Instalação passo a passo
*Siga TODAS as etapas abaixo após concluir a leitura anterior*

## 1. Preparação <a name="preparation"></a>
**1 - Baixar a ISO: _Vá no_ [site oficial do Arch Linux](https://archlinux.org/download/) _e faça o baixe o arquivo `.iso`_**. _É o instalador do sistema._

**2 - Fazer o Backup do Pendrive: _Salve todos os seus arquivos importantes que estejam dentro do pendrive em outro lugar, o Ventoy irá formatar o pendrive apagando tudo nele presente._**

**3 - Baixar o Ventoy: _Baixe o programa pelo_ [site oficial do Ventoy](https://www.ventoy.net/en/download.html).** _É a ferramenta que prepara o pendrive para que o computador possa iniciar o instalador do Arch Linux (a ISO)._

**4 - Criar o pendrive bootável: _Apenas assista esse_ [tutorial de 3 minutos](https://www.youtube.com/watch?v=3HVAM1M3fQU) _pra criar o pendrive bootável pelo Ventoy._**
> Se você terminou essa parte, pode seguir pro terceiro passo.

## 2. BIOS Configuration <a name="bios"></a>
**1 - Entre na BIOS:** _Agora, vamos **reinicie o computador. Assim que começar a ligar, aperte rapidamente e repetidamente a tecla de acesso à BIOS (geralmente F2 ou Delete)** até abrir uma tela diferente._

**2 - Ordem de boot:** _Dentro dessa tela, **procure a opção `boot` e altere a ordem para que seu pendrive seja o primeiro a ser lido. Salve as alterações e saia usando F10 e Enter;** o computador reiniciará e carregará o instalador do Arch Linux._ 
> Se você terminou essa parte também, siga pro passo três.

## 3. Configuração Básica do Ambiente Live <a name="live"></a>
> **IMPORTANTE: É comum que ao executar comandos válidos, o terminal não retorne nenhuma confirmação ou mensagem de acerto. No Linux, o padrão é que o sistema exiba mensagens somente se tiver um erro. Se você executar um comando e não receber nenhuma mensagem de volta e parecer vazio para você digitar novamente, significa que o comando funcionou.** 

> Se a fonte do terminal estiver muito pequena pra enxergar, use o comando `setfont ter-132b` pra aumentar o tamanho das letras.

**1 - Configurar o teclado:** _A primeira coisa pra fazer é_ **_configurar o teclado para que as teclas pressionadas saiam corretamente na tela (como o sinal de / ou : ou ?). Se o seu teclado tiver a tecla Ç, ela é o padrão brasileiro (br-abnt2). Caso contrário, ele geralmente será o americano (US)._** _Digite o comando que_ **_corresponde ao seu modelo de teclado usando o comando:_ `loadkeys (seu layout)`_. No caso,_ `loadkeys br-abnt2` _ou_ `loadkeys us`_._**

> **Se a sua internet for conectada por cabo de rede, pule o próximo passo.**

**2 - Conectar-se à Internet: _Se você não estiver usando cabo de rede, digite o comando_ `iwctl` _pra abrir o configurador do Wi-Fi chamado `IWD`._** _Quando digitar esse comando, você verá que a linha de comando terá mudado e entrado no programa._ **_Precisamos identificar o nome da sua placa de rede com o comando_ `device list`_; aparecerá algo como `wlan0` ou algo semelhante._** _Depois,_ **_usaremos os comandos_ `station [nome do seu dispositivo] scan` _e_ `station [nome do seu dispositivo] get-networks` _para escanear e mostrar as redes encontradas, e por último o comando_ `station [nome do seu dispositivo] connect [nome da sua rede]` _para conectar na rede, e então, digite a senha. Depois disso, digite_ `exit` _pra sair do programa `IWD`._**

> **Exemplo: `station wlan0 scan`, `station wla0 get-networks` _and_ `station wlan0 connect wifi01`.**

> Teste a conexão usando o comando `ping -c 3 google.com`, **se o terminal retornar em millisegundos, está conectado, e você pode pular a resolução de DNS.** Caso contrário, você pode ter um problema de DNS e pode resolvê-lo aqui embaixo.

<details>
    <summary><i>Resolução de DNS</i></summary>

_Pra resolver esse erro, você precisa_ **_abrir e editar o arquivo_ `resolv.conf` _com o editor de texto NANO_**_, que já está instalado por padrão nesse ambiente Live._ **_Usaremos o comando_ `nano/etc/resolv.conf` _pra abrir o arquivo. Em seguida, exclua tudo nele e digite_ `nameserver 1.1.1.1`_, pressione Enter para ir até a linha debaixo e digite também_ `nameserver 8.8.8.8`_. Use CTRL+O pra salvar o arquivo (confirme com Enter) e, pra sair do NANO, aperte CTRL+X.**_
> Agora teste novamente com `ping -c 3 google.com`, se retornar em millisegundos, tá tudo certo.
</details><br>

**3 - Sincronizar o relógio: _Para que a instalação ocorra corretamente, precisamos sincronizar o relógio do ambiente Live com o horário padrão global, para isso usaremos apenas o comando_ `timedatectl set-ntp true` _._**

**4 - Corrigir as Keyrings:** _A utilidade principal delas é garantir que os pacotes que você baixa são oficiais e não foram alterados por hackers. Pra isso, precisamos_ **_remover as keyrings antigas usando o comando_ `rm -rf/etc/pacman.d/gnupg`.** _Agora, vamos_ **_iniciar as novas keyrings com os comandos_ `pacman-key --init` _e_ `pacman-key --populate archlinux` _para carregar essas keyrings. Em seguida, use o comando_ `pacman -Sy archlinux-keyring --noconfirm` _para atualizar o pacote de keyrings_**_, baixando-o diretamente dos servidores Arch._
> Se tiver terminado aqui, passe pro estágio quatro.

## 4. Particionamento <a name="partitioning"></a>
> **Antes de particionarmos o disco, precisamos saber se seu computador inicia no modo UEFI ou no modo BIOS (Legacy). Para isso, usaremos o comando `ls/sys/firmware/efi/` para saber se o sistema está no modo UEFI. Se o comando retornar alguns nomes de pastas, o modo UEFI estará ativo. Se não retornar nada, está no modo BIOS (Legacy). Com esses dados você pode prosseguir para o tutorial sobre seu modo de inicialização.**
<details>
    <summary><b>4.1 - UEFI</b></summary>

**1 - Procurar pelo seu disco:** _Para particionar o disco, primeiro_ **_precisamos saber o nome do dispositivo desejado. Para fazer isso, use o comando_ `lsblk` _para exibir todos os dispositivos de armazenamento presentes na sua máquina._** _Em seguida,_ **_procure o disco que você formatará, se for um SSD SATA ou HD, ele aparecerá como `vda` ou `sda`; se for um SSD NVMe, será algo como `nvme0n1`._**

**2 - Formatar o disco:** _Após encontrar o dispositivo desejado (por exemplo, usarei `sda` como o disco escolhido),_ **_use o comando_ `cfdisk /dev/[seu dispositivo]` _— ou seja_ `cfdisk /dev/sda` _—, selecione_ `gpt` _e pressione Enter. Em seguida, navegue por todas as partições existentes e use a opção_ `Delete` _em cada uma, para que reste apenas uma linha com o dispositivo chamado_ `Free Space`_._**

**3 - Dividir o espaço: _Agora, vá para_ `New`_, uma mensagem aparecerá solicitando que você insira o tamanho desejado, escreva 1G_**_, pressione Enter,_ **_depois, vá em_ `Type` _e escolha a opção_ `EFI System`** _(essa será a partição de boot). Em seguida,_ **_selecione o_ `Free Space` _novamente usando a seta para baixo, pressione a opção_ `New` _novamente e adicione um tamanho para o_ `swap`** _(memória auxiliar caso a RAM acabe) — é recomendado usar a mesma quantidade de gigabytes da sua RAM — pressione Enter,_ **_vá em_ `Type` _e selecione_ `Linux Swap`_._** _Depois,_ **_selecione_ `Free Space` _novamente, clique em_ `Novo`_, selecione todo o espaço restante, vá em_ `Type` e escolha a opção `Linux filesystem`_._** _Ao fazer tudo isso,_ **_vá para_ `Write`**_, pressione Enter e_ **_digite_ `Yes` _para salvar as alterações; para sair, apenas use a opção_ `Sair` _e pressione Enter._**

**4 - Preparar as Partições: _Use_ `mkfs.ext4 /dev/sda3` _para formatar a partição_ `root` _do sistema em `EXT4`. Use também_ `mkfs.fat -F 32 /dev/sda1` _para formatar a partição de inicialização (se você for fazer dualboot, não faça essa formatação). O comando_ `mkswap /dev/sda2` _para formatar a partição de `swap` e o comando_ `swapon /dev/sda2` _para ativar o_ `swap`_._**

**5 - Montagem: _Use o comando_ `mount /dev/[seu dispositivo]3 /mnt` _— ou seja_ `mount /dev/sda3 /mnt` _— para montar a_ `root` _do seu sistema_** _(a partição raiz)._ **_Use o comando_ `mkdir -p /mnt/boot/efi` _para criar a pasta `/mnt/boot/efi` que será montada a seguir._** _Finalmente,_ **_use_ `mount /dev/sda1/mnt/boot/efi` _para montar a partição de inicialização em seu devido lugar._**
</details>
<details>
    <summary><b>4.2 - BIOS</b></summary>
    
</details>

> Se ja tiver formatado e montado o disco, vá pro quinto passo.

## 5. Installation of the base <a name="base-install"></a>
**1 - Update the Mirrors:** _We will use Reflector, which is a tool that serves to choose and organize the best mirrors (servers) for you to download the packages. For this,_ **_we will use the command_ `reflector --country [Your country] --latest 20 --sort rate --verbose --save/etc/pacman.d/mirrorlist`_. Once the program runs, the mirrors will already be up to date._**
> **Example: `reflector --country Brazil --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`**

**2 - Install the Linux kernel and essential packages: _Use the command_ `pacstrap /mnt base linux linux-firmware base-devel` _to install the system base and some additional necessary packages._**

**3 - Generate assembly configuration file: _Using_ `genfstab -U/mnt >> /mnt/etc/fstab` _to create the automatic assembly file of the parts._**

> If you have finished these stages, go to step 6.

## 6. Initial System Settings <a name="iss"></a>
> **Now that the base files have been installed, we need to "get inside" your new system to configure it from the inside. To do this, we will use the command `arch-chroot /mnt` and voila: we are inside the system. Inside co `chroot`, we will follow with the settings.**

**1 - Installing Essential Programs: _We will install these fundamental programs for our installation on Arch:_**

**- Network Manager: _Responsible for managing network connections and allowing us to connect to wifi (Wi-Fi and Ethernet)._**<br>
**- Sudo: _Allows regular users to execute commands with administrator (_`root`_) privileges._**<br>
**- Grub: _The bootloader that allows the operating system to start._**<br>
**- Efi Boot Manager: _Tool to interact with and manage boot inputs in EFI/UEFI firmware._**<br>
**- Os Prober: _Used by Grub to detect other operating systems (such as Windows) on disk. Just install os-prober if you are going to dualboot (two operating systems on the same machine)._**<br>
**- Nano: _Plain text editor via terminal to edit configuration files._**<br>
**- Kitty: _A modern, fast and highly customizable terminal emulator._**<br>
**- Hyprland: _The Wayland-based windowing environment (Window Manager) that will be your graphical interface._**<br>
**- Xorg Xwayland: _Ensures compatibility of legacy applications (X11) within the Wayland environment._**<br>
**- Xdg Desktop Portal Hyprland: _Allows the Hyprland environment to communicate with applications (required for prints and screen sharing)._**<br>
**- Mesa: _Open source drivers for graphics acceleration (essential for video operation)._**<br>
**- Pipewire / Pipewire pulse: _Modern audio servers that manage the system's sound._**<br>
**- Alsa Utils: _Set of basic tools for sound setup and testing._**<br>
**- Fonts: _(Fira Code, Comfortaa, Emoji): Packages of letters and icons necessary for the system to display text and emojis correctly._**<br>
**- Ly: _A lightweight and minimalist login manager (Display Manager) for you to enter your password when turning on your PC._**<br>

> **It's worth remembering that you have 100% freedom to choose your own programs. Since this tutorial reflects the way I like to configure my system, we will use these packages, but feel free to exchange them if you feel it is necessary.**

**Command:**

> **First, use `pacman -Syu` to update the entire system before installing the applications. Finally, use the --noconfirm parameter to automatically answer YES if you want to proceed with the installation. Don't forget to put the program names all in detail and with a dash (-) so that there are no errors during installation.**

    pacman -S networkmanager sudo grub efibootmgr os-prober nano kitty hyprland xorg-xwayland xdg-desktop-portal-hyprland mesa pipewire pipewire-pulse alsa-utils ttf-fira-code ttf-comfortaa noto-fonts-emoji ly --noconfirm

**2 - Activate Important Library: _Run the_ `nano/etc/pacman.conf` _command to open the_ `pacman.conf`** _file with the NANO text editor. With the file open_ **_use CTRL+F to search for_ `multilib`_, remove the_ `#` _from lines containing:_**

    #[multilib]
    #include = /etc/pacman.d/mirrorlist
**_At the end:_**

    [multilib]
    include = /etc/pacman.d/mirrorlist

_then_ **_use CTRL+O to save the file (confirm with Enter) and, to exit NANO, press CTRL+X._**

**3 - Location, Keyboard and Hostname: _We will use the command_ `ln -sf/usr/share/zoneinfo/[your continent]/[your city] /etc/localtime`** _— in my case,_ `ln -sf/usr/share/zoneinfo/America/Sao_Paulo/etc/localtime` _— and,_ **_then the command_ `hwclock --systohc` _to synchronize the system time with that of your region._**
> **Tip: Note that on the way to the city, if the name is compound (like São Paulo), you should use the underline (`_`) instead of space.**

_We will also use the_ **_command_ `nano/etc/locale.gen` _to open the_ file `locale.gen`**_. In it,_ **_we will uncomment the system language by removing the # from the desired language_**_._ **_Use the CTRL+F shortcut to search for your language_** _— for example,_ **`en_US` _or_ `pt_BR`** _— and_ **_remove o_ `#` _from the lines containing your language_**_, as in the example below:_

    #en_US.UTF-8 UTF-8
**_or_**

    #pt_BR.UTF-8 UTF-8
**_At the end:_**

    en_US.UTF-8 UTF-8
**_or_**

    pt_BR.UTF-8 UTF-8

_Then_ **_use CTRL+O to save the file (confirm with Enter) and, to exit NANO, press CTRL+X. Next, we will use the_ `locale-gen` _command to generate the previously uncommented languages._**
> **Note: You can download more than one language, but you will only have to choose one to be displayed as the main one.**

_Now we will_ **_apply the chosen main language to the system using the command_ `echo "LANG=[your language]" > /etc/locale.conf`.** _In my case, the command is_ `echo "LANG=en_US.UTF-8" > /etc/locale.conf`_, but it could also be_ `echo "LANG=pt_BR.UTF-8" > /etc/locale.conf`_._
> **Note: Remember that the language you put here must be the same as the one you uncommented in the previous step inside the locale.gen file.**

_After that, we will_ **_configure the keyboard layout and put a name for the computer. To add the layout for the keyboard we use_ `echo "KEYMAP=[your layout]" > /etc/vconsole.conf`.** _In my case, the command is_ `echo "KEYMAP=br-abnt2" > /etc/vconsole.conf`_, but it could also be_ `echo "KEYMAP=us" > /etc/vconsole.conf`_._

_Now_ **_for the computer name we use_ `echo "[name]" > /etc/hostname`_,_** _I will use_ `echo "secura" > /etc/hostname`_._
> **Note: The name you choose for the `hostname` is how your computer will appear on the network and terminal.**

**4 - Configure the Bootloader:** _For GRUB configuration (downloaded in step 6.1 - Installing Essential Programs), we will perform custom adjustments the way I like GRUB._ **_Use the command_ `nano/etc/default/grub` _to access the configuration file_**_; locate the third line and_ **_change the value of_ `GRUB_TIMEOUT=5` _to_ `-1`_, resulting in_ `GRUB_TIMEOUT=-1`**_, so that the countdown is disabled and the system does not automatically start, so that we can choose which system we want to start._ **_If you intend to perform the dualboot, you will need to navigate to the last line of the file and uncomment the_ `GRUB_DISABLE_OS_PROBER=false` _option, removing the_ `#` _character. use CTRL+O to save the file (confirm with Enter) and, to exit NANO, press CTRL+X._**

_Then,_ **_install grub on the partition using the command_ `grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB` _and generate the final configuration files using the command_ `grub-mkconfig -o/boot/grub/grub.cfg`_._**

**5 - User Creation and Permissions:** _To proceed with system security, the next step is to define access credentials. First, it is necessary_ **_to configure a password for the administrator user (`root`) through the command_ `passwd`_; when you run it, the system will ask you to enter and confirm the desired password_** _(it is strongly recommended that this password be unique and different from the one that will be used in your personal account)._

_Next,_ **_to create your user, we will use the command_ `useradd -m -g wheel -s/bin/bash [your_user]`** _— in my case, the command is_ `useradd -m -g wheel -s/bin/bash guihnxz`_. Finally,_ **_to assign a password to this new user, use the command_ `passwd [your_user]`**_, which in my example would be_ `passwd guihnxz`_, remembering again to define a different combination than the one used for_ `root`_._

_Now,_ **_use the command_ `EDITOR=nano visudo` _to open the_ `sudoers` _file through the NANO editor (note to write EDITOR in capital letters). Use the shortcut CTRL+F to search for the_ `wheel`_term and remove the_ `#` _character from the line_ `#%wheel ALL=(ALL:ALL) ALL`_._** _This change will allow your user to use the sudo command to gain administrator privileges by requesting the user's password before each run._


## 7 - Activate Services and Finish the Installation <a name="final"></a>
**1 - Activate Services:** _For the system to operate correctly after restarting, it is necessary to enable two essential services so that they start automatically. First,_ **_activate the Network Manager service with the command_ `systemctl enable NetworkManager`**_, ensuring that Wi-Fi can be configured properly. Then,_ **_enable the Ly service_** _(the Display Manager installed in step 6.1)_ **_using the command_ `systemctl enable ly.service`_, so that the login interface is loaded instead of just the default terminal._**

# Fim
## Pronto, seu sistema é utilizável e tem o básico configurado para você personalizar como desejar. Não foi assim tão ruim assim né? Rsrs
