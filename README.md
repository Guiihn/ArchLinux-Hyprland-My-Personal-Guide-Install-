<h1><b>My Install Guide for Arch Linux & Hyprland</b></h1>
<h3><i>This is my personal guide to installing this system. <br>
These will be the paths we will follow:</i></h3>
<details>
    <summary><b><i>1. Preparation </i></b></summary>

In this step, we will focus on preparing the environment for **downloading the ISO**, **prepare your bootable USB stick**, **downloading Ventoy** (the program that will be **used to create the bootable USB stick**) and then **creating the bootable USB stick**.
</details>

<details>
    <summary><b><i>2. BIOS/UEFI Setup </i></b></summary>

**We will put the bootable USB stick to start first.**
</details>

<details>
    <summary><b><i>3. Basic Live Environment Setup </i></b></summary>

In this stage we will do some **basic configurations in the Live environment**. These settings will be to **configure keyboard layout, internet, synchronize the clock and keyring correction.**
</details>

<details>
    <summary><b><i>4. Disk Partitioning </i></b></summary>

In this step, the focus will be on **organizing your disk** (HD or SSD). Let's **decide how the space will be divided, defining the boot partition for the system to start, the swap partition** (auxiliary memory) to help the computer when it is overloaded, **and the main partition** where all the files and the operating system will be stored.
</details>

<details>
    <summary><b><i>5. Installation of the Base </i></b></summary>

Here we will **update the mirrors and also use `pacstrap` to install the Linux Kernel** and essential packages. Finally, let's **generate the fstab file with `genfstab` to remember and automatically mount the partitions** that you had previously mounted in step 4.
</details>

<details>
    <summary><b><i>6. Initial System Settings </i></b></summary>

In this step, we will focus on the **internal configuration of the system**. We will **activate an important library called multilib, configure the time zone, location, language and hostname, download and configure the bootloader** (GRUB) so that it starts correctly, and finally, we will **perform user creation with administrator privileges**.
</details> 

<details>
    <summary><b><i>7. Activate Services and Finish the Installation </i></b></summary>

Finally, we will **activate services so that the system starts correctly** and we will **disassemble the live environment and restart the machine.**
</details> 

---

<h2><b> 📢 IMPORTANT NOTICE </h2></b>
<h3><i>This tutorial is designed to be easy to understand and execute. However, to ensure successful installation and avoid system failures, it is essential that all instructions are strictly followed. Make sure you type the commands exactly as described, respecting spaces, capital letters, and special characters.</i></h3>

---
# 🛠️ Step-by-Step Installation
*Follow ALL sections below after completing the previous reading.*

## 1. Preparation <a name="preparation"></a>
**1 - Download the ISO: _Go to the_ [official Arch Linux website](https://archlinux.org/download/) _and download the installation file (ISO)_**. _He is the "installer" of the system._

**2 - Pendrive Backup: _Save ALL your USB stick files elsewhere. The next step will erase everything in it._**

**3 - Download Ventoy: _Download the program from_ [Ventoy's official website](https://www.ventoy.net/en/download.html).** _It is the tool that prepares the USB stick so that the computer can start the Arch Linux installer._

**4 - Create the Installation Pendrive: _Just follow this_ [3 minute tutorial](https://www.youtube.com/watch?v=3HVAM1M3fQU) _to create the bootable USB stick._**
> If you have finished these stages, go to step 2.

## 2. BIOS Configuration <a name="bios"></a>
**1 - Access the BIOS/UEFI:** _Now, let's **restart the computer. Once it starts turning on, keep pressing the setup key (usually F2 or Delete)** until it opens a different screen._

**2 - Boot Order:** _Within that screen, **look for the Boot option and change the order so that your USB stick is first so that it is read first. Save the changes and exit;** the computer will restart and load the Arch Linux installer._ 
> If you have finished these stages, go to step 3.

## 3. Basic configuration of the Live Environment <a name="live"></a>
> **IMPORTANT: It is common that when executing valid commands, the terminal does not return any confirmation or hit messages. On Linux, the default is for the system to only display messages if an error occurs. If you run a command and the bottom line just appears empty for you to type again, it means the command worked.** 

> If the terminal source is small use the command `setfont ter-132b` to increase the size of the letters.

**1 - Configure Keyboard:** _The first thing to do is **set up the keyboard so that the keys you press come out correctly on the screen (like the / or - sign). If your keyboard has the Ç key, it is the Brazilian standard (br-abnt2). If he doesn't, he's usually the American (us).**_ _Enter the command that **corresponds to your keyboard model using the command:**_ **`loadkeys (your model)`. _That is_ `loadkeys br-abnt2` _or_ `loadkeys us`.**

> If you connect your computer to the internet via cable, you don't need to do this next step.

**2 - Internet Connection: _If you are not using network cable, enter the command below to open the Wi-Fi configurator named IWD:_ `iwctl`.** _When you type this command, you will see that the command line will have changed and entered the program._ **_We need to identify the name of your network card with the_ `device list` _command; something like wlan0 or something similar should appear._** _Next,_ **_we will use the commands_ `station [your device name] scan` _and_ `station [your device name] get-networks` _to scan and display the found networks, and then the command_ `station [your device name] connect [your internet name]` to connect, and then enter the password. After this, type `exit` to exit the program.**

> **Examples: `station wlan0 scan`, `station wla0 get-networks` _and_ `station wlan0 connect wifi01`.**

> Test the connection using `ping -c 3 google.com`, **if it returns in milliseconds, it will be connected, and can skip step DNS Resolution** Otherwise you may have a DNS issue and you can resolve it below.

<details>
    <summary><i>DNS Resolution</i></summary>

_To resolve this error, we need to **open and edit the resolv.conf file with the NANO text editor**, which is already installed by default in this Live environment. **We will use the `nano/etc/resolv.conf` command to open the file. Then delete everything in it and type `nameserver 1.1.1.1`, press Enter to go to the bottom line and also type `nameserver 8.8.8.8`. Use CTRL+O to save the file (confirm with Enter) and, to exit NANO, press CTRL+X.**_
> Test again using `ping -c 3 google.com`, if it returns in milliseconds, everything is fine.
</details><br>

**3 - Environment Clock: _For the installation to go properly, we need to synchronize the clock and live environment with global standard time, for this we will only use the_ `timedatectl set-ntp true` _command._**

**4 - Keyring Correction:** _First, we need to_ **_remove the old keyrings using the command_ `rm -rf/etc/pacman.d/gnupg`.** _Now, let's_ **_start the new keyrings with the_ `pacman-key --init` _and_ `pacman-key --populate archlinux` _commands to load these keys. Then use the command_ `pacman -Sy archlinux-keyring --noconfirm` _to update the keyring package_**_, downloading it directly from Arch servers._
> If you have finished these stages, go to step 4.

## 4. Disk Partitioning <a name="partitioning"></a>
> **Before we partition your disk, we need to know your computer starts in UEFI mode or BIOS (Legacy) mode. To do this, we will use the command `ls/sys/firmware/efi/` to know if the system is in UEFI mode. If the command returns some folder names, UEFI mode is active. If it doesn't return anything, it's in BIOS (Legacy). With this data you can proceed to the tutorial regarding its initialization mode.**
<details>
    <summary><b>4.1 - UEFI</b></summary>

**1 - Search for your disk:** _To partition your disk, we first_ **_need to know the name of the desired device. To do this, use the_ `lsblk` _command to display all storage devices present on your machine. Then look for the disk you will format:_** _if it is a HD or SATA SSD, it should appear as `vda` or `sda`; if it is an NVMe SSD, it will be something like `nvme0n1`._

**2 - Cleaning Disk:** _After finding the desired device (for example, I will use `sda` as the chosen disk),_ **_use the command_ `cfdisk/dev/[your device]` _— i.e._ `cfdisk/dev/sda` _—, select_ `gpt` _and press Enter. Then, browse through all existing partitions and use the_ `Delete` _option on each one, so that only one line remains with the device called Free space._**

**3 - Split the Device: _Now, go to_ `New`_, a message will appear asking you to enter the desired size, write 1G_**_, press Enter,_ **_select_ `Type` _and choose the_ `EFI System` _option_** _(this will be the boot partition). Then,_ **_select_ `Free Space` _again using the down arrow, press_ `New` _again, and add a size for Swap_** _(auxiliary memory in case RAM runs out) — it's recommended to use the same amount of gigabytes of your RAM — press Enter,_ **_go to_ `Type`_, and select_ `Linux Swap`_._** _Then,_ **_select_ `Free Space` _again, click_ `New`_, select all remaining space and leave it in_ `Linux filesystem`_._** _As you do all this,_ **_go to_ `Write`**_, press Enter, and_ **_type_ `yes` _to save the changes_**_; to exit, simply_ **_go to_ `Quit` _and press Enter._**

**4 - Prepare the Partitions: _Use_ `mkfs.ext4/dev/sda3` _to format the system_ `root` _partition in EXT4. Also use `mkfs.fat -F 32/dev/sda1` to format the boot partition (If you are going to do dualboot, do not do this formatting). The command_ `mkswap/dev/sda2` _to format the partition_ `sda2` _as swap and the command_ `swapon/dev/sda2` _to activate swap._**

**5 - Assembly: _Use the command_ `mount/dev/[your device]3 /mnt` _— i.e._ `mount/dev/sda3 /mnt` _— to mount the_ `root` _of your system. Use the command_ `mkdir -p/mnt/boot/efi` _to create the folder that will be mounted next._** _Finally,_ **_use_ `mount/dev/sda1/mnt/boot/efi` _to mount the boot partition in its proper place._**

</details>
<details>
    <summary><b>4.2 - BIOS</b></summary>
    
</details>

> If you have finished these stages, go to step 5.

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

# The End
## That's it, your system is usable and has the basics set up for you to customize as you wish. It wasn't that bad, was it? lol
