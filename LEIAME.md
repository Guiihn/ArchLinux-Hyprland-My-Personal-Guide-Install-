<h1><b>Meu Guia de Instalação do Arch Linux & Hyprland</b></h1>
<h3><i>Esse é o meu guia pessoal pra instalar esse sistema. <br>
Esses serão os caminhos que vamos seguir:</i></h3>
<details>
    <summary><b><i>1. Preparação </i></b></summary>

Nesse passo, vamos focar em preparar o ambiente pra **baixar a ISO**, **preparar o seu pendrive bootável**, **baixar o Ventoy** (o programa que vai ser **usado pra criar o pendrive bootável**) e depois **criar o pendrive bootável**.
</details>

<details>
    <summary><b><i>2. Configuração de BIOS/UEFI </i></b></summary>

**Vamos colocar o pendrive bootável pra iniciar primeiro.**
</details>

<details>
    <summary><b><i>3. Configuração Básica do Ambiente Live </i></b></summary>

Nessa fase, vamos fazer algumas **configurações básicas no ambiente Live**. Essas configurações vão ser pra **configurar o layout do teclado, internet, sincronizar o relógio e correção do chaveiro (keyring).**
</details>

<details>
    <summary><b><i>4. Particionamento de Disco </i></b></summary>

Nesse passo, o foco vai ser em **organizar o seu disco** (HD ou SSD). Vamos **decidir como o espaço vai ser dividido, definindo a partição de boot pro sistema iniciar, a partição swap** (memória auxiliar) pra ajudar o computador quando ele estiver sobrecarregado, **e a partição principal** onde todos os arquivos e o sistema operacional vão ficar guardados.
</details>

<details>
    <summary><b><i>5. Instalação da Base </i></b></summary>

Aqui vamos **atualizar os mirrors e também usar o `pacstrap` pra instalar o Kernel Linux** e pacotes essenciais. Finalmente, vamos **gerar o arquivo fstab com o `genfstab` pra lembrar e montar automaticamente as partições** que você tinha montado anteriormente no passo 4.
</details>

<details>
    <summary><b><i>6. Configurações Iniciais do Sistema </i></b></summary>

Nesse passo, vamos focar na **configuração interna do sistema**. Vamos **ativar uma biblioteca importante chamada multilib, configurar o fuso horário, localidade, idioma e hostname, baixar e configurar o bootloader** (GRUB) pra que ele inicie corretamente e, por fim, vamos **fazer a criação do usuário com privilégios de administrador**.
</details> 

<details>
    <summary><b><i>7. Ativar Serviços e Finalizar a Instalação </i></b></summary>

Finalmente, vamos **ativar os serviços pra que o sistema inicie corretamente** e vamos **desmontar o ambiente live e reiniciar a máquina.**
</details> 


---


##  **📢 AVISO IMPORTANTE**


### *Esse tutorial foi projetado pra ser fácil de entender e executar. No entanto, pra garantir o sucesso da instalação e evitar falhas no sistema, é essencial que todas as instruções sejam seguidas rigorosamente. Certifique-se de digitar os comandos exatamente como estão descritos, respeitando espaços, letras maiúsculas e caracteres especiais.*


---


# 🛠️ Instalação Passo a Passo


*Siga TODAS as seções abaixo após completar a leitura anterior.*


## 1. Preparação


**1 - Baixe a ISO: *Vá até o* [site oficial do Arch Linux](https://archlinux.org/download/) *e baixe o arquivo de instalação (ISO)***. *Ele é o "instalador" do sistema.*


**2 - Backup do Pendrive: *Salve TODOS os arquivos do seu pendrive em outro lugar. O próximo passo vai apagar tudo que estiver nele.***


**3 - Baixe o Ventoy: *Baixe o programa no* [site oficial do Ventoy](https://www.ventoy.net/en/download.html).** *É a ferramenta que prepara o pendrive pra que o computador consiga iniciar o instalador do Arch Linux.*


**4 - Crie o Pendrive de Instalação: *Basta seguir esse* [tutorial de 3 minutos](https://www.youtube.com/watch?v=3HVAM1M3fQU) *pra criar o pendrive bootável.***



> 
> Se você finalizou essas etapas, vá pro passo 2.
> 
> 
> 


## 2. Configuração de BIOS


**1 - Acessar a BIOS/UEFI:** *Agora, vamos **reiniciar o computador. Assim que ele começar a ligar, fique pressionando a tecla de setup (geralmente F2 ou Delete)** até abrir uma tela diferente.*


**2 - Ordem de Boot:** *Dentro dessa tela, **procure pela opção Boot e mude a ordem pra que o seu pendrive fique em primeiro pra ser lido primeiro. Salve as alterações e saia;** o computador vai reiniciar e carregar o instalador do Arch Linux.*



> 
> Se você finalizou essas etapas, vá pro passo 3.
> 
> 
> 


## 3. Configuração básica do Ambiente Live



> 
> **IMPORTANTE: É comum que, ao executar comandos válidos, o terminal não retorne nenhuma mensagem de confirmação ou sucesso. No Linux, o padrão é o sistema exibir mensagens apenas se ocorrer um erro. Se você rodar um comando e a linha de baixo apenas aparecer vazia pra você digitar de novo, significa que o comando funcionou.**
> 
> 
> 



> 
> Se a fonte do terminal estiver pequena, use o comando `setfont ter-132b` pra aumentar o tamanho das letras.
> 
> 
> 


**1 - Configurar Teclado:** *A primeira coisa a se fazer é **configurar o teclado pra que as teclas que você pressiona saiam corretamente na tela (como o sinal de / ou -). Se o seu teclado tem a tecla Ç, ele é o padrão brasileiro (br-abnt2). Se não tem, ele geralmente é o americano (us).*** *Insira o comando que **corresponde ao seu modelo de teclado usando o comando:*** **`loadkeys (seu modelo)`. *Ou seja,* `loadkeys br-abnt2` *ou* `loadkeys us`.**



> 
> Se você conectar seu computador na internet via cabo, não precisa fazer esse próximo passo.
> 
> 
> 


**2 - Conexão de Internet: *Se você não estiver usando cabo de rede, insira o comando abaixo pra abrir o configurador de Wi-Fi chamado IWD:* `iwctl`.** *Quando você digitar esse comando, vai ver que a linha de comando vai ter mudado e entrado no programa.* ***Precisamos identificar o nome da sua placa de rede com o comando* `device list`*; algo como wlan0 ou algo similar deve aparecer.*** *Em seguida,* ***vamos usar os comandos* `station [nome do seu dispositivo] scan` *e* `station [nome do seu dispositivo] get-networks` *pra escanear e exibir as redes encontradas, e então o comando* `station [nome do seu dispositivo] connect [nome da sua internet]` pra conectar, e depois insira a senha. Após isso, digite `exit` pra sair do programa.**



> 
> **Exemplos: `station wlan0 scan`, `station wla0 get-networks` *e* `station wlan0 connect wifi01`.**
> 
> 
> 



> 
> Teste a conexão usando `ping -c 3 google.com`, **se retornar em milissegundos, vai estar conectado, e pode pular o passo de Resolução de DNS.** Caso contrário, você pode estar com um problema de DNS e pode resolver abaixo.
> 
> 
> 


<details>
    <summary><i>Resolução de DNS</i></summary>


*Pra resolver esse erro, precisamos **abrir e editar o arquivo resolv.conf com o editor de texto NANO**, que já vem instalado por padrão nesse ambiente Live. **Vamos usar o comando `nano /etc/resolv.conf` pra abrir o arquivo. Apague tudo que estiver nele e digite `nameserver 1.1.1.1`, aperte Enter pra ir pra linha de baixo e digite também `nameserver 8.8.8.8`. Use CTRL+O pra salvar o arquivo (confirme com Enter) e, pra sair do NANO, pressione CTRL+X.***
> 
> Teste novamente usando `ping -c 3 google.com`, se retornar em milissegundos, está tudo certo.
> 
> 
> 
</details>


**3 - Relógio do Ambiente: *Pra que a instalação ocorra devidamente, precisamos sincronizar o relógio do ambiente live com o horário padrão global. Pra isso, usaremos apenas o comando* `timedatectl set-ntp true`*.***


**4 - Correção do Chaveiro:** *Primeiro, precisamos* ***remover os chaveiros antigos usando o comando* `rm -rf /etc/pacman.d/gnupg`**. *Agora vamos* ***iniciar os novos chaveiros com os comandos* `pacman-key --init` *e* `pacman-key --populate archlinux` *pra carregar essas chaves. Em seguida, use o comando* `pacman -Sy archlinux-keyring --noconfirm` *pra atualizar o pacote de chaveiros***, *baixando ele diretamente dos servidores do Arch.*



> 
> Se você finalizou essas etapas, vá pro passo 4.
> 
> 
> 


## 4. Particionamento de Disco



> 
> **Antes de particionarmos o seu disco, precisamos saber se o seu computador inicia em modo UEFI ou modo BIOS (Legacy). Pra isso, use o comando `ls /sys/firmware/efi/` pra verificar o UEFI. Se o comando retornar vários nomes de pastas, o modo UEFI está ativo. Se não retornar nada, está em BIOS (Legacy). Com esse dado, você pode prosseguir pro tutorial referente ao seu modo de boot.**
> 
> 
> 


<details>
    <summary><b>4.1 - UEFI</b></summary>


**1 - Identificar seu disco:** *Pra particionar o seu disco, primeiro* ***precisamos saber o nome do dispositivo desejado. Pra isso, use o comando* `lsblk` *pra exibir todos os dispositivos de armazenamento presentes na sua máquina. Procure pelo disco que você irá formatar:*** *se for um HD ou SSD SATA, ele deve aparecer como `sda` ou `sdb`; se for um SSD NVMe, vai ser algo como `nvme0n1`.*


**2 - Limpando o Disco:** *Após encontrar o dispositivo desejado (por exemplo, usarei o `sda`),* ***use o comando* `cfdisk /dev/[seu dispositivo]` *— ou seja,* `cfdisk /dev/sda` —, *selecione* `gpt` *e aperte Enter. Em seguida, navegue por todas as partições existentes e use a opção* `Delete` *em cada uma delas, pra que reste apenas uma linha com o dispositivo chamado Free space.***


**3 - Dividir o Dispositivo: *Agora, vá em* `New`*, uma mensagem aparecerá pedindo pra inserir o tamanho desejado; escreva 1G***, *aperte Enter,* ***selecione* `Type` *e escolha a opção* `EFI System`*** *(essa vai ser a partição de boot). Em seguida,* ***selecione* `Free Space` *novamente usando a seta pra baixo, aperte* `New` *de novo e adicione um tamanho pra Swap*** *(memória auxiliar caso a RAM acabe) — é recomendado usar a mesma quantidade de gigas da sua memória RAM — aperte Enter,* ***vá em* `Type` *e selecione* `Linux Swap`***. *Por fim,* ***selecione* `Free Space` *mais uma vez, clique em* `New`*, selecione todo o espaço restante e deixe em* `Linux filesystem`***. *Ao terminar tudo isso,* ***vá em* `Write`***, *aperte Enter e* ***digite* `yes` *pra salvar as alterações***; *pra sair, basta* ***ir em* `Quit` *e apertar Enter.***


**4 - Preparar as Partições: *Use* `mkfs.ext4 /dev/sda3` *pra formatar a partição* `root` *do sistema em EXT4. Use também `mkfs.fat -F 32 /dev/sda1` pra formatar a partição de boot (Se você for fazer dualboot, não faça essa formatação). O comando* `mkswap /dev/sda2` *pra formatar a sda2 como swap e o comando* `swapon /dev/sda2` *pra ativar a swap.***


**5 - Montagem: *Use o comando* `mount /dev/[seu dispositivo]3 /mnt` *— ou seja,* `mount /dev/sda3 /mnt` — *pra montar a* `root` *do seu sistema. Use o comando* `mkdir -p /mnt/boot/efi` *pra criar a pasta onde a partição de boot será montada em seguida.*** *Por fim,* ***use* `mount /dev/sda1 /mnt/boot/efi` *pra montar a partição de boot no seu devido lugar.***


</details>
<details>
    <summary><b>4.2 - BIOS</b></summary>
    


</details>



> 
> Se você finalizou essas etapas, vá pro passo 5.
> 
> 
> 


## 5. Instalação da base


**1 - Atualizar os Mirrors:** *Vamos usar o Reflector, que é uma ferramenta que serve pra escolher e organizar os melhores mirrors (servidores) pra você baixar os pacotes. Pra isso,* ***usaremos o comando* `reflector --country [Seu país] --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`*. Assim que o programa rodar, os seus mirrors já vão estar atualizados.***
> 
> **Exemplo: `reflector --country Brazil --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`**
> 
> 
> 


**2 - Instale o kernel Linux e pacotes essenciais: *Use o comando* `pacstrap /mnt base linux linux-firmware base-devel` *pra instalar a base do sistema e também alguns pacotes adicionais necessários.***


**3 - Gerar arquivo fstab: *Use* `genfstab -U /mnt >> /mnt/etc/fstab` *pra criar o arquivo de montagem automática das partições.***



> 
> Se você finalizou essas etapas, vá pro passo 6.
> 
> 
> 


## 6. Configurações Iniciais do Sistema



> 
> **Agora que os arquivos base foram instalados, precisamos "entrar" no seu novo sistema pra configurar ele por dentro. Pra isso, vamos usar o comando `arch-chroot /mnt` e prontinho: estamos dentro do sistema. Dentro do `chroot`, vamos seguir com as configurações.**
> 
> 
> 


**1 - Instalando Programas Essenciais: *Vamos instalar esses programas fundamentais pra nossa instalação no Arch:***


**- Network Manager: *Responsável por gerenciar as conexões de rede e permitir que a gente se conecte ao wifi (Wi-Fi e Ethernet).***

**- Sudo: *Permite que usuários comuns executem comandos com privilégios de administrador (*`root`*) .***

**- Grub: *O bootloader que permite que o sistema operacional inicie.***

**- Efi Boot Manager: *Ferramenta pra interagir e gerenciar entradas de boot no firmware EFI/UEFI.***

**- Os Prober: *Utilizado pelo Grub pra detectar outros sistemas operacionais (como o Windows) no disco. Instale o os-prober apenas se for fazer dualboot (dois sistemas operacionais na mesma máquina).***

**- Nano: *Editor de texto simples via terminal pra editar arquivos de configuração.***

**- Kitty: *Um emulador de terminal moderno, rápido e altamente customizável.***

**- Hyprland: *O ambiente de janelas baseado em Wayland (Window Manager) que vai ser sua interface gráfica.***

**- Xorg Xwayland: *Garante a compatibilidade de aplicações legadas (X11) dentro do ambiente Wayland.***

**- Xdg Desktop Portal Hyprland: *Permite que o ambiente Hyprland se comunique com aplicações (necessário pra prints e compartilhamento de tela).***

**- Mesa: *Drivers de código aberto pra aceleração gráfica (essencial pro funcionamento do vídeo).***

**- Pipewire / Pipewire pulse: *Servidores de áudio modernos que gerenciam o som do sistema.***

**- Alsa Utils: *Conjunto de ferramentas básicas pra configuração e teste de som.***

**- Fontes: *(Fira Code, Comfortaa, Emoji): Pacotes de letras e ícones necessários pro sistema exibir textos e emojis corretamente.***

**- Ly: *Um gerenciador de login (Display Manager) leve e minimalista pra você inserir sua senha ao ligar o PC.***




> 
> **Vale lembrar que você tem 100% de liberdade pra escolher seus próprios programas. Como esse tutorial reflete a maneira que eu gosto de configurar o meu sistema, vamos usar esses pacotes, mas sinta-se à vontade pra trocá-los se julgar necessário.**
> 
> 
> 


**Comando:**



> 
> **Primeiro, use `pacman -Syu` pra atualizar todo o sistema antes de instalar as aplicações. Por fim, use o parâmetro --noconfirm pra responder automaticamente SIM pra se deseja prosseguir com a instalação. Não esqueça de colocar os nomes dos programas todos detalhadamente e com traço (-) pra que não ocorram erros na instalação.**
> 
> 
>
