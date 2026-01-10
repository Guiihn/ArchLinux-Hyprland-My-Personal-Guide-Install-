<h1><b>Meu Guia de Instalação do Arch Linux & Hyprland</b></h1>
<h3><i>Este é o meu guia pessoal para a instalação deste sistema. <br>
Estes são os caminhos que seguiremos:</i></h3>
<details>
    <summary><b><i>1. Preparação </i></b></summary>

Nesta etapa, focaremos na preparação do ambiente: **download da ISO**, **preparação do seu pendrive bootável**, **download do Ventoy** (o programa que será **usado para criar o pendrive bootável**) e, finalmente, a **gravação da imagem no pendrive**.
</details>

<details>
    <summary><b><i>2. Configuração de BIOS/UEFI </i></b></summary>

**Configuraremos o pendrive bootável para iniciar como prioridade na ordem de boot.**
</details>

<details>
    <summary><b><i>3. Configuração Básica do Ambiente Live </i></b></summary>

Nesta fase, faremos algumas **configurações básicas no ambiente Live**. Essas definições servirão para **configurar o layout do teclado, internet, sincronizar o relógio e corrigir o chaveiro (keyring).**
</details>

<details>
    <summary><b><i>4. Particionamento de Disco </i></b></summary>

Neste passo, o foco será a **organização do seu disco** (HD ou SSD). Vamos **decidir como o espaço será dividido, definindo a partição de boot para o sistema iniciar, a partição swap** (memória auxiliar) para ajudar o computador em sobrecarga, **e a partição principal** onde todos os arquivos e o sistema operacional ficarão armazenados.
</details>

<details>
    <summary><b><i>5. Instalação da Base </i></b></summary>

Aqui, vamos **atualizar os mirrors (espelhos) e utilizar o `pacstrap` para instalar o Kernel Linux** e pacotes essenciais. Por fim, vamos **gerar o arquivo fstab com o `genfstab` para que o sistema lembre e monte automaticamente as partições** que você configurou no passo 4.
</details>

<details>
    <summary><b><i>6. Configurações Iniciais do Sistema </i></b></summary>

Nesta etapa, focaremos na **configuração interna do sistema**. Vamos **ativar a importante biblioteca multilib, configurar fuso horário, localidade, idioma e hostname, baixar e configurar o bootloader** (GRUB) para que ele inicie corretamente e, por fim, faremos a **criação do usuário com privilégios de administrador**.
</details> 

<details>
    <summary><b><i>7. Ativar Serviços e Finalizar a Instalação </i></b></summary>

Finalmente, vamos **ativar os serviços necessários para que o sistema inicie corretamente**, **desmontaremos o ambiente live e reiniciaremos a máquina.**
</details> 

---

<h2><b> 📢 AVISO IMPORTANTE </h2></b>
<h3><i>Este tutorial foi desenvolvido para ser fácil de entender e executar. No entanto, para garantir o sucesso da instalação e evitar falhas no sistema, é essencial que todas as instruções sejam seguidas rigorosamente. Certifique-se de digitar os comandos exatamente como descritos, respeitando espaços, letras maiúsculas e caracteres especiais.</i></h3>

---
# 🛠️ Instalação Passo a Passo
*Siga TODAS as seções abaixo após concluir a leitura anterior.*

## 1. Preparação <a name="preparation"></a>
**1 - Download da ISO: _Acesse o_ [site oficial do Arch Linux](https://archlinux.org/download/) _e baixe o arquivo de instalação (ISO)_**. _Ele é o "instalador" do sistema._

**2 - Backup do Pendrive: _Salve TODOS os arquivos do seu pendrive em outro lugar. O próximo passo apagará tudo o que estiver nele._**

**3 - Download do Ventoy: _Baixe o programa no_ [site oficial do Ventoy](https://www.ventoy.net/en/download.html).** _Ele é a ferramenta que prepara o pendrive para que o computador consiga iniciar o instalador do Arch Linux._

**4 - Criar o Pendrive de Instalação: _Basta seguir este_ [tutorial de 3 minutos](https://www.youtube.com/watch?v=3HVAM1M3fQU) _para criar o seu pendrive bootável._**
> Se você concluiu estas etapas, prossiga para o passo 2.

## 2. Configuração de BIOS <a name="bios"></a>
**1 - Acessar a BIOS/UEFI:** _Agora, vamos **reiniciar o computador. Assim que ele começar a ligar, mantenha pressionada a tecla de setup (geralmente F2 ou Delete)** até que uma tela diferente seja aberta._

**2 - Ordem de Boot:** _Dentro dessa tela, **procure pela opção Boot e altere a ordem para que o seu pendrive fique em primeiro lugar. Salve as alterações e saia;** o computador reiniciará e carregará o instalador do Arch Linux._ 
> Se você concluiu estas etapas, prossiga para o passo 3.

## 3. Configuração básica do Ambiente Live <a name="live"></a>
> **IMPORTANTE: É comum que, ao executar comandos válidos, o terminal não retorne nenhuma mensagem de confirmação ou sucesso. No Linux, o padrão é que o sistema exiba mensagens apenas se ocorrer um erro. Se você rodar um comando e a linha de baixo aparecer vazia para você digitar novamente, significa que o comando funcionou.** > Se a fonte do terminal estiver pequena, utilize o comando `setfont ter-132b` para aumentar o tamanho das letras.

**1 - Configurar Teclado:** _A primeira coisa a se fazer é **configurar o teclado para que as teclas que você pressiona saiam corretamente na tela (como o sinal de / ou -). Se o seu teclado possui a tecla Ç, ele é o padrão brasileiro (br-abnt2). Se não possui, geralmente é o americano (us).**_ _Insira o comando que **corresponde ao seu modelo de teclado utilizando o comando:**_ **`loadkeys (seu modelo)`. _Ou seja,_ `loadkeys br-abnt2` _ou_ `loadkeys us`.**

> Se você conectar seu computador à internet via cabo, não precisa realizar este próximo passo.

**2 - Conexão de Internet: _Se você não estiver usando cabo de rede, insira o comando abaixo para abrir o configurador de Wi-Fi chamado IWD:_ `iwctl`.** _Ao digitar este comando, você verá que a linha de comando mudará, indicando que entrou no programa._ **_Precisamos identificar o nome da sua placa de rede com o comando_ `device list`; _algo como wlan0 ou similar deve aparecer._** _Em seguida,_ **_usaremos os comandos_ `station [nome do seu dispositivo] scan` _e_ `station [nome do seu dispositivo] get-networks` _para escanear e exibir as redes encontradas, e então o comando_ `station [nome do seu dispositivo] connect [nome da sua internet]` para conectar, inserindo a senha em seguida. Após isso, digite `exit` para sair do programa.**

> **Exemplos: `station wlan0 scan`, `station wlan0 get-networks` _e_ `station wlan0 connect wifi01`.**

> Teste a conexão usando `ping -c 3 google.com`. **Se retornar em milissegundos, você está conectado e pode pular a etapa de Resolução de DNS.** Caso contrário, você pode estar com um problema de DNS e pode resolvê-lo abaixo.

<details>
    <summary><i>Resolução de DNS</i></summary>

_Para resolver este erro, precisamos **abrir e editar o arquivo resolv.conf com o editor de texto NANO**, que já vem instalado por padrão neste ambiente Live. **Usaremos o comando `nano /etc/resolv.conf` para abrir o arquivo. Apague tudo o que estiver nele e digite `nameserver 1.1.1.1`, aperte Enter para ir para a linha de baixo e digite também `nameserver 8.8.8.8`. Use CTRL+O para salvar o arquivo (confirme com Enter) e, para sair do NANO, pressione CTRL+X.**_
> Teste novamente usando `ping -c 3 google.com`; se retornar em milissegundos, está tudo certo.
</details>

**3 - Relógio do Ambiente: _Para que a instalação ocorra devidamente, precisamos sincronizar o relógio do ambiente live com o horário padrão global. Para isso, usaremos apenas o comando_ `timedatectl set-ntp true`._**

**4 - Correção do Chaveiro (Keyring):** _Primeiro, precisamos_ **_remover os chaveiros antigos usando o comando_ `rm -rf /etc/pacman.d/gnupg`.** _Agora, vamos_ **_iniciar os novos chaveiros com os comandos_ `pacman-key --init` _e_ `pacman-key --populate archlinux` _para carregar estas chaves. Em seguida, use o comando_ `pacman -Sy archlinux-keyring --noconfirm` _para atualizar o pacote de chaveiros_**_, baixando-o diretamente dos servidores do Arch._
> Se você concluiu estas etapas, prossiga para o passo 4.

## 4. Particionamento de Disco <a name="partitioning"></a>
> **Antes de particionarmos seu disco, precisamos saber se seu computador inicia em modo UEFI ou modo BIOS (Legacy). Para isso, usaremos o comando `ls /sys/firmware/efi/` para verificar se o sistema está em modo UEFI. Se o comando retornar alguns nomes de pastas, o modo UEFI está ativo. Se não retornar nada, está em BIOS (Legacy). Com este dado, você pode prosseguir para o tutorial referente ao seu modo de inicialização.**
<details>
    <summary><b>4.1 - UEFI</b></summary>

**1 - Identificar seu disco:** _Para particionar seu disco, primeiro_ **_precisamos saber o nome do dispositivo desejado. Para isso, use o comando_ `lsblk` _para exibir todos os dispositivos de armazenamento presentes na sua máquina. Procure pelo disco que você irá formatar:_** _se for um HD ou SSD SATA, ele deve aparecer como `sda` ou `sdb`; se for um SSD NVMe, será algo como `nvme0n1`._

**2 - Limpando o Disco:** _Após encontrar o dispositivo desejado (por exemplo, usarei o `sda`),_ **_use o comando_ `cfdisk /dev/[seu dispositivo]` _— ou seja,_ `cfdisk /dev/sda` —, _selecione_ `gpt` _e aperte Enter. Em seguida, navegue por todas as partições existentes e use a opção_ `Delete` _em cada uma delas, para que reste apenas uma linha com o dispositivo chamado Free space (Espaço Livre)._**

**3 - Dividir o Dispositivo: _Agora, vá em_ `New`_, uma mensagem aparecerá pedindo para inserir o tamanho desejado; escreva 1G_**_, aperte Enter,_ **_selecione_ `Type` _e escolha a opção_ `EFI System`** _(esta será a partição de boot). Em seguida,_ **_selecione_ `Free Space` _novamente usando a seta para baixo, aperte_ `New` _de novo e adicione um tamanho para a Swap_** _(memória auxiliar caso a RAM acabe) — é recomendado usar a mesma quantidade de gigabytes da sua memória RAM — aperte Enter,_ **_vá em_ `Type` _e selecione_ `Linux Swap`_._** _Depois,_ **_selecione_ `Free Space` _mais uma vez, clique em_ `New`_, selecione todo o espaço restante e deixe em_ `Linux filesystem`_._** _Ao terminar tudo isso,_ **_vá em_ `Write`**, _aperte Enter e_ **_digite_ `yes` _para salvar as alterações_**_; para sair, basta_ **_ir em_ `Quit` _e apertar Enter._**

**4 - Preparar as Partições: _Use_ `mkfs.ext4 /dev/sda3` _para formatar a partição_ `root` _do sistema em EXT4. Use também `mkfs.fat -F 32 /dev/sda1` para formatar a partição de boot (Se você for fazer dualboot, não faça esta formatação). O comando_ `mkswap /dev/sda2` _para formatar a partição_ `sda2` _como swap e o comando_ `swapon /dev/sda2` _para ativar a swap._**

**5 - Montagem: _Use o comando_ `mount /dev/[seu dispositivo]3 /mnt` _— ou seja,_ `mount /dev/sda3 /mnt` — _para montar a_ `root` _do seu sistema. Use o comando_ `mkdir -p /mnt/boot/efi` _para criar a pasta onde a partição de boot será montada em seguida._** _Por fim,_ **_use_ `mount /dev/sda1 /mnt/boot/efi` _para montar a partição de boot em seu devido lugar._**

</details>
<details>
    <summary><b>4.2 - BIOS</b></summary>
    

</details>

> Se você concluiu estas etapas, prossiga para o passo 5.

## 5. Instalação da base <a name="base-install"></a>
**1 - Atualizar os Mirrors:** _Usaremos o Reflector, que é uma ferramenta que serve para escolher e organizar os melhores mirrors (servidores) para você baixar os pacotes. Para isso,_ **_usaremos o comando_ `reflector --country [Seu país] --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`_. Assim que o programa rodar, os mirrors já estarão atualizados._**
> **Exemplo: `reflector --country Brazil --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`**

**2 - Instalar o kernel Linux e pacotes essenciais: _Use o comando_ `pacstrap /mnt base linux linux-firmware base-devel` _para instalar a base do sistema e alguns pacotes adicionais necessários._**

**3 - Gerar arquivo de configuração de montagem: _Utilize o comando_ `genfstab -U /mnt >> /mnt/etc/fstab` _para criar o arquivo de montagem automática das partições._**

> Se você concluiu estas etapas, prossiga para o passo 6.

## 6. Configurações Iniciais do Sistema <a name="iss"></a>
> **Agora que os arquivos base foram instalados, precisamos "entrar" no seu novo sistema para configurá-lo por dentro. Para isso, usaremos o comando `arch-chroot /mnt`. Dentro do ambiente `chroot`, seguiremos com as configurações.**

**1 - Instalando Programas Essenciais: _Instalaremos estes programas fundamentais para a nossa instalação no Arch:_**

**- Network Manager: _Responsável por gerenciar as conexões de rede e permitir que nos conectemos ao Wi-Fi e Ethernet._**<br>
**- Sudo: _Permite que usuários comuns executem comandos com privilégios de administrador (_`root`_)._**<br>
**- Grub: _O bootloader que permite que o sistema operacional inicie._**<br>
**- Efi Boot Manager: _Ferramenta para interagir e gerenciar entradas de boot no firmware EFI/UEFI._**<br>
**- Os Prober: _Utilizado pelo Grub para detectar outros sistemas operacionais (como o Windows) no disco. Instale apenas se for fazer dualboot._**<br>
**- Nano: _Editor de texto simples via terminal para editar arquivos de configuração._**<br>
**- Kitty: _Um emulador de terminal moderno, rápido e altamente customizável._**<br>
**- Hyprland: _O ambiente de janelas baseado em Wayland (Window Manager) que será sua interface gráfica._**<br>
**- Xorg Xwayland: _Garante a compatibilidade de aplicações legadas (X11) dentro do ambiente Wayland._**<br>
**- Xdg Desktop Portal Hyprland: _Permite que o ambiente Hyprland se comunique com aplicações (necessário para prints e compartilhamento de tela)._**<br>
**- Mesa: _Drivers de código aberto para aceleração gráfica (essenciais para o funcionamento do vídeo)._**<br>
**- Pipewire / Pipewire pulse: _Servidores de áudio modernos que gerenciam o som do sistema._**<br>
**- Alsa Utils: _Conjunto de ferramentas básicas para configuração e teste de som._**<br>
**- Fonts: _(Fira Code, Comfortaa, Emoji): Pacotes de letras e ícones necessários para o sistema exibir textos e emojis corretamente._**<br>
**- Ly: _Um gerenciador de login (Display Manager) leve e minimalista para você inserir sua senha ao ligar o PC._**<br>

> **Vale lembrar que você tem 100% de liberdade para escolher seus próprios programas. Como este tutorial reflete a maneira que eu gosto de configurar meu sistema, usaremos estes pacotes, mas sinta-se à vontade para trocá-los se julgar necessário.**

**Comando:**

> **Primeiro, use `pacman -Syu` para atualizar todo o sistema antes de instalar as aplicações. Por fim, use o parâmetro --noconfirm para responder automaticamente SIM para prosseguir com a instalação. Não esqueça de colocar os nomes dos programas detalhadamente para que não ocorram erros.**

    pacman -S networkmanager sudo grub efibootmgr os-prober nano kitty hyprland xorg-xwayland xdg-desktop-portal-hyprland mesa pipewire pipewire-pulse alsa-utils ttf-fira-code ttf-comfortaa noto-fonts-emoji ly --noconfirm

**2 - Ativar Biblioteca Importante: _Execute o comando_ `nano /etc/pacman.conf` _para abrir o arquivo de configuração do pacman. Com o arquivo aberto,_ **_use CTRL+W para procurar por_ `multilib`_ e remova o_ `#` _das linhas contendo:_**

    #[multilib]
    #include = /etc/pacman.d/mirrorlist
**_Ao final:_**

    [multilib]
    include = /etc/pacman.d/mirrorlist

_Depois,_ **_use CTRL+O para salvar o arquivo (confirme com Enter) e, para sair do NANO, pressione CTRL+X._**

**3 - Localidade, Teclado e Hostname: _Usaremos o comando_ `ln -sf /usr/share/zoneinfo/[seu continente]/[sua cidade] /etc/localtime`** _— no meu caso,_ `ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime` — _e,_ **_em seguida, o comando_ `hwclock --systohc` _para sincronizar a hora do sistema com a da sua região._**
> **Dica: Note que no caminho da cidade, se o nome for composto (como São Paulo), você deve usar o underline (`_`) no lugar do espaço.**

_Também usaremos o_ **_comando_ `nano /etc/locale.gen` _para abrir o arquivo_ `locale.gen`**. Nele,_ **_descomentaremos o idioma do sistema removendo o # do idioma desejado_**_._ **_Utilize o atalho CTRL+W para pesquisar pelo seu idioma_** _— por exemplo,_ **`en_US` _ou_ `pt_BR`** _— e_ **_remova o_ `#` _das linhas que contêm o seu idioma_**_, como no exemplo abaixo:_

    #pt_BR.UTF-8 UTF-8
**_Ao final:_**

    pt_BR.UTF-8 UTF-8

_Então,_ **_use CTRL+O para salvar o arquivo (confirme com Enter) e, para sair do NANO, pressione CTRL+X. Em seguida, usaremos o comando_ `locale-gen` _para gerar os idiomas descomentados anteriormente._**
> **Nota: Você pode habilitar mais de um idioma, mas terá que escolher apenas um para ser exibido como o principal.**

_Agora vamos_ **_aplicar o idioma principal escolhido ao sistema utilizando o comando_ `echo "LANG=[seu idioma].UTF-8" > /etc/locale.conf`.** _No meu caso:_ `echo "LANG=pt_BR.UTF-8" > /etc/locale.conf`_ ou _ `echo "LANG=en_US.UTF-8" > /etc/locale.conf`_._
> **Nota: Lembre-se que o idioma que você colocar aqui deve ser o mesmo que você descomentou no passo anterior dentro do arquivo locale.gen.**

_Depois disso, vamos_ **_configurar o layout do teclado e colocar um nome para o computador. Para adicionar o layout para o teclado usamos_ `echo "KEYMAP=[seu layout]" > /etc/vconsole.conf`.** _No meu caso:_ `echo "KEYMAP=br-abnt2" > /etc/vconsole.conf`_ ou _ `echo "KEYMAP=us" > /etc/vconsole.conf`_._

_Agora_ **_para o nome do computador usamos_ `echo "[nome]" > /etc/hostname`_,_** _eu usarei_ `echo "secura" > /etc/hostname`_._
> **Nota: O nome que você escolher para o `hostname` é como o seu computador aparecerá na rede e no terminal.**

**4 - Configurar o Bootloader:** _Para a configuração do GRUB (baixado no passo 6.1), faremos alguns ajustes personalizados. **Use o comando `nano /etc/default/grub` para acessar o arquivo**; localize a terceira linha e **altere o valor de `GRUB_TIMEOUT=5` para `-1`**. O resultado será **`GRUB_TIMEOUT=-1`**, desativando a contagem regressiva para que o sistema espere sua escolha manual. **Se você for fazer dualboot, vá até a última linha e descomente a opção `GRUB_DISABLE_OS_PROBER=false` removendo o `#`. Salve com CTRL+O e saia com CTRL+X.**_

_Em seguida,_ **_instale o grub na partição utilizando o comando_ `grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB` _e gere os arquivos de configuração final com o comando_ `grub-mkconfig -o /boot/grub/grub.cfg`_._**

**5 - Criação de Usuário e Permissões:** _Para prosseguir com a segurança do sistema, o próximo passo é definir as credenciais de acesso. Primeiro, é necessário_ **_configurar uma senha para o usuário administrador (`root`) através do comando_ `passwd`; quando executá-lo, o sistema solicitará que você digite e confirme a senha desejada** _(recomenda-se fortemente que esta senha seja única e diferente da que será usada em sua conta pessoal)._

_Em seguida,_ **_para a criação do seu usuário, utilizaremos o comando_ `useradd -m -g wheel -s /bin/bash [seu_usuario]`** _— no meu caso,_ `useradd -m -g wheel -s /bin/bash guihnxz`_. Por fim,_ **_para atribuir uma senha a este novo usuário, utilize o comando_ `passwd [seu_usuario]`**_ (ex: _ `passwd guihnxz`_)_.

_Agora,_ **_utilize o comando_ `EDITOR=nano visudo` _para abrir o arquivo `sudoers` pelo editor NANO (lembre-se de escrever EDITOR em letra maiúscula). Utilize o atalho CTRL+W para pesquisar pelo termo `wheel` e remova o_ `#` _da linha_ `%wheel ALL=(ALL:ALL) ALL`_._** _Esta alteração permitirá que o seu usuário utilize o comando sudo para obter privilégios de administrador, solicitando a sua senha antes de cada execução._


## 7 - Ativar Serviços e Finalizar a Instalação <a name="final"></a>
**1 - Ativar Serviços:** _Para que o sistema opere corretamente após reiniciar, é necessário ativar dois serviços essenciais para que iniciem automaticamente. Primeiro,_ **_ative o serviço do Network Manager com o comando_ `systemctl enable NetworkManager`**_, garantindo que o Wi-Fi possa ser configurado. Depois,_ **_ative o serviço do Ly_** _(o Display Manager instalado no passo 6.1)_ **_usando o comando_ `systemctl enable ly.service`_, para que a interface de login seja carregada ao invés do terminal padrão._**

# Fim
## É isso, seu sistema está utilizável e com o básico configurado para você customizar como quiser. Não foi tão ruim, né? kkk
