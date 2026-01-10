<h1><b>My Install Guide for Arch Linux & Hyprland</b></h1>
<h3><i>This is my personal guide to installing this system. <br>
These are the steps we will follow:</i></h3>
<details>
    <summary><b><i>1. Preparation </i></b></summary>

In this step, we will focus on environment preparation: **downloading the ISO**, **preparing your bootable USB drive**, **downloading Ventoy** (the tool **used to create the bootable media**), and finally **flashing the ISO to the USB stick**.
</details>

<details>
    <summary><b><i>2. BIOS/UEFI Setup </i></b></summary>

**We will configure the bootable USB drive to take priority in the boot order.**
</details>

<details>
    <summary><b><i>3. Basic Live Environment Setup </i></b></summary>

In this stage, we will perform **basic configurations within the Live environment**. These settings include **configuring the keyboard layout, internet connectivity, clock synchronization, and keyring fixes.**
</details>

<details>
    <summary><b><i>4. Disk Partitioning </i></b></summary>

In this step, we focus on **organizing your disk** (HDD or SSD). We will **decide how space is allocated, defining the boot partition for system startup, the swap partition** (virtual memory) to assist the system under heavy load, **and the main partition** where the operating system and all files will be stored.
</details>

<details>
    <summary><b><i>5. Base Installation </i></b></summary>

Here we will **update the mirrors and use `pacstrap` to install the Linux Kernel** and essential packages. Finally, we will **generate the fstab file using `genfstab` to ensure the system remembers and automatically mounts the partitions** you configured in step 4.
</details>

<details>
    <summary><b><i>6. Initial System Settings </i></b></summary>

In this step, we will focus on **internal system configuration**. We will **enable the important multilib repository, configure the time zone, locale, language, and hostname, then download and configure the bootloader** (GRUB) to ensure a proper boot. Finally, we will **create a user with administrator privileges**.
</details> 

<details>
    <summary><b><i>7. Enable Services and Finish Installation </i></b></summary>

Lastly, we will **enable the necessary services for the system to start correctly**, then **unmount the live environment and reboot the machine.**
</details> 

---

<h2><b> 📢 IMPORTANT NOTICE </h2></b>
<h3><i>This tutorial is designed to be easy to understand and execute. However, to ensure a successful installation and avoid system failures, it is essential that all instructions are followed strictly. Make sure to type the commands exactly as described, respecting spaces, capital letters, and special characters.</i></h3>

---
# 🛠️ Step-by-Step Installation
*Follow ALL sections below after completing the previous reading.*

## 1. Preparation <a name="preparation"></a>
**1 - Download the ISO: _Go to the_ [official Arch Linux website](https://archlinux.org/download/) _and download the installation file (ISO)_**. _This is the system "installer."_

**2 - USB Drive Backup: _Save ALL files from your USB stick elsewhere. The next step will erase everything on it._**

**3 - Download Ventoy: _Download the program from_ [Ventoy's official website](https://www.ventoy.net/en/download.html).** _This is the tool that prepares the USB stick so the computer can boot the Arch Linux installer._

**4 - Create the Installation Media: _Simply follow this_ [3-minute tutorial](https://www.youtube.com/watch?v=3HVAM1M3fQU) _to create your bootable USB drive._**
> If you have finished these stages, go to step 2.

## 2. BIOS Configuration <a name="bios"></a>
**1 - Access BIOS/UEFI:** _Now, let's **reboot the computer. As it starts to power on, keep pressing the setup key (usually F2 or Delete)** until the firmware screen appears._

**2 - Boot Order:** _Within that screen, **look for the Boot options and change the order so your USB stick is listed first. Save the changes and exit;** the computer will restart and load the Arch Linux installer._ 
> If you have finished these stages, go to step 3.

## 3. Basic configuration of the Live Environment <a name="live"></a>
> **IMPORTANT: It is common for the terminal to not return any confirmation or success messages after executing valid commands. On Linux, the default behavior is for the system to only display messages if an error occurs. If you run a command and the next line simply appears empty for you to type again, it means the command worked.** > If the terminal font is too small, use the command `setfont ter-132b` to increase the letter size.

**1 - Configure Keyboard:** _The first step is to **set up the keyboard so that the keys you press correspond correctly to what appears on the screen (such as / or -). If your keyboard has the Ç key, it follows the Brazilian standard (br-abnt2). If it doesn't, it is usually the American standard (us).**_ _Enter the command that **corresponds to your keyboard model using the command:**_ **`loadkeys (your model)`. _For example,_ `loadkeys br-abnt2` _or_ `loadkeys us`.**

> If you are connecting to the internet via an Ethernet cable, you can skip the next step.

**2 - Internet Connection: _If you are not using a network cable, enter the command below to open the Wi-Fi configurator named IWD:_ `iwctl`.** _Once you type this command, you will notice the prompt has changed, indicating you are inside the program._ **_We need to identify your network interface using the_ `device list` _command; it should appear as wlan0 or something similar._** _Next,_ **_we will use the commands_ `station [your device name] scan` _and_ `station [your device name] get-networks` _to scan and list available networks, followed by the command_ `station [your device name] connect [your internet name]` to connect, then enter your password. After this, type `exit` to leave the program.**

> **Examples: `station wlan0 scan`, `station wlan0 get-networks` _and_ `station wlan0 connect wifi01`.**

> Test your connection using `ping -c 3 google.com`. **If it returns a response in milliseconds, you are connected and can skip the DNS Resolution step.** Otherwise, you may have a DNS issue which can be resolved below.

<details>
    <summary><i>DNS Resolution</i></summary>

_To resolve this error, we need to **open and edit the resolv.conf file using the NANO text editor**, which is installed by default in the Live environment. **We will use the `nano /etc/resolv.conf` command to open the file. Delete its entire contents and type `nameserver 1.1.1.1`, press Enter to go to the next line and type `nameserver 8.8.8.8`. Use CTRL+O to save (confirm with Enter) and CTRL+X to exit NANO.**_
> Test again using `ping -c 3 google.com`; if it returns a response, you are all set.
</details>

**3 - Environment Clock: _For the installation to proceed correctly, we must synchronize the Live environment's clock with global standard time. To do this, simply use the_ `timedatectl set-ntp true` _command._**

**4 - Keyring Correction:** _First, we need to_ **_remove the old keyrings using the command_ `rm -rf /etc/pacman.d/gnupg`.** _Now, let's_ **_initialize the new keyrings with the_ `pacman-key --init` _and_ `pacman-key --populate archlinux` _commands to load the keys. Then, use the command_ `pacman -Sy archlinux-keyring --noconfirm` _to update the keyring package_**_, downloading it directly from the Arch servers._
> If you have finished these stages, go to step 4.

## 4. Disk Partitioning <a name="partitioning"></a>
> **Before partitioning your disk, we need to know if your computer boots in UEFI mode or BIOS (Legacy) mode. To do this, use the command `ls /sys/firmware/efi/` to check for UEFI. If the command returns several folder names, UEFI mode is active. If it returns nothing, the system is in BIOS (Legacy). With this information, you can proceed to the tutorial corresponding to your boot mode.**
<details>
    <summary><b>4.1 - UEFI</b></summary>

**1 - Identify your disk:** _To partition your disk, we first_ **_need to identify the device name. Use the_ `lsblk` _command to display all storage devices on your machine. Look for the disk you intend to format:_** _if it is an HDD or SATA SSD, it should appear as `sda` or `sdb`; if it is an NVMe SSD, it will be something like `nvme0n1`._

**2 - Cleaning the Disk:** _Once you have found the correct device (for example, I will use `sda`),_ **_use the command_ `cfdisk /dev/[your device]` _— e.g._ `cfdisk /dev/sda` —,_ select_ `gpt` _and press Enter. Then, navigate through all existing partitions and use the_ `Delete` _option on each one until only a single line labeled Free space remains._**

**3 - Partition the Device: _Now, go to_ `New`_, and when prompted for the size, enter 1G_**_, press Enter,_ **_select_ `Type` _and choose the_ `EFI System` _option_** _(this will be the boot partition). Next,_ **_select_ `Free Space` _again using the down arrow, press_ `New`_, and set a size for Swap_** _(auxiliary memory) — it is recommended to match the amount of RAM you have — press Enter,_ **_go to_ `Type`_, and select_ `Linux Swap`_._** _Finally,_ **_select_ `Free Space` _once more, click_ `New`_, and allocate the remaining space to_ `Linux filesystem`_._** _Once finished,_ **_go to_ `Write`**_, press Enter, and_ **_type_ `yes` _to save the changes_**_; to finish, simply_ **_go to_ `Quit` _and press Enter._**

**4 - Prepare the Partitions: _Use_ `mkfs.ext4 /dev/sda3` _to format the system_ `root` _partition in EXT4. Use `mkfs.fat -F 32 /dev/sda1` to format the boot partition (If you are planning a dual-boot, do not format this partition). Run_ `mkswap /dev/sda2` _to format_ `sda2` _as swap and_ `swapon /dev/sda2` _to activate it._**

**5 - Mounting: _Use the command_ `mount /dev/[your device]3 /mnt` _— e.g._ `mount /dev/sda3 /mnt` —_ to mount the system_ `root`_. Use_ `mkdir -p /mnt/boot/efi` _to create the mount point for the boot partition._** _Finally,_ **_use_ `mount /dev/sda1 /mnt/boot/efi` _to mount the boot partition in its correct place._**

</details>
<details>
    <summary><b>4.2 - BIOS</b></summary>
    

</details>

> If you have finished these stages, go to step 5.

## 5. Installation of the base <a name="base-install"></a>
**1 - Update the Mirrors:** _We will use Reflector, a tool designed to select and organize the best mirrors (servers) for downloading packages. For this,_ **_we will use the command_ `reflector --country [Your country] --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`_. Once the process finishes, your mirrors will be optimized._**
> **Example: `reflector --country Brazil --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`**

**2 - Install the Linux kernel and essential packages: _Use the command_ `pacstrap /mnt base linux linux-firmware base-devel` _to install the system base and necessary development tools._**

**3 - Generate the fstab file: _Use_ `genfstab -U /mnt >> /mnt/etc/fstab` _to create the file that manages automatic partition mounting._**

> If you have finished these stages, go to step 6.

## 6. Initial System Settings <a name="iss"></a>
> **Now that the base files are installed, we need to "enter" your new system to configure it from the inside. We will use the command `arch-chroot /mnt`. Inside the `chroot` environment, we will proceed with the settings.**

**1 - Installing Essential Programs: _We will install the fundamental programs for this Arch setup:_**

**- Network Manager: _Manages network connections and enables Wi-Fi and Ethernet connectivity._**<br>
**- Sudo: _Allows regular users to execute commands with administrator (_`root`_) privileges._**<br>
**- Grub: _The bootloader that enables the operating system to start._**<br>
**- Efi Boot Manager: _A tool to manage boot entries in EFI/UEFI firmware._**<br>
**- Os Prober: _Used by Grub to detect other operating systems (like Windows). Install this only for dual-boot configurations._**<br>
**- Nano: _A terminal-based text editor for modifying configuration files._**<br>
**- Kitty: _A modern, fast, and highly customizable terminal emulator._**<br>
**- Hyprland: _The Wayland-based window manager that will serve as your graphical interface._**<br>
**- Xorg Xwayland: _Ensures compatibility for legacy X11 applications within Wayland._**<br>
**- Xdg Desktop Portal Hyprland: _Enables communication between the environment and applications (required for screenshots and screen sharing)._**<br>
**- Mesa: _Open-source drivers for graphics acceleration._**<br>
**- Pipewire / Pipewire pulse: _Modern audio servers for system sound management._**<br>
**- Alsa Utils: _A set of tools for sound configuration and testing._**<br>
**- Fonts: _(Fira Code, Comfortaa, Emoji): Essential icon and text packages for correct rendering._**<br>
**- Ly: _A lightweight and minimalist login manager (Display Manager) for entering your password at startup._**<br>

> **Note that you have 100% freedom to choose your own programs. This tutorial reflects my personal configuration preferences, but feel free to swap packages as you see fit.**

**Command:**

> **First, run `pacman -Syu` to update the system before installation. Use the --noconfirm parameter to automatically accept the installation prompts. Ensure program names are typed correctly to avoid errors.**

    pacman -S networkmanager sudo grub efibootmgr os-prober nano kitty hyprland xorg-xwayland xdg-desktop-portal-hyprland mesa pipewire pipewire-pulse alsa-utils ttf-fira-code ttf-comfortaa noto-fonts-emoji ly --noconfirm

**2 - Enable Important Repository: _Run_ `nano /etc/pacman.conf` _to open the configuration file. Use CTRL+W to search for_ `multilib`_, and remove the_ `#` _from the following lines:_**

    #[multilib]
    #include = /etc/pacman.d/mirrorlist
**_It should look like this:_**

    [multilib]
    include = /etc/pacman.d/mirrorlist

_Then_ **_use CTRL+O to save (confirm with Enter) and CTRL+X to exit NANO._**

**3 - Locale, Keyboard, and Hostname: _We will use the command_ `ln -sf /usr/share/zoneinfo/[your continent]/[your city] /etc/localtime`** _— e.g._ `ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime` —_ followed by_ **_the command_ `hwclock --systohc` _to sync the system clock with your region._**
> **Tip: If your city has a compound name (like Sao Paulo), use an underscore (`_`) instead of a space.**

_Next,_ **_open the locale.gen file with_ `nano /etc/locale.gen`**. _In this file,_ **_uncomment your desired system language by removing the #_**_._ **_Use CTRL+W to search for your language_** _— e.g.,_ **`en_US` _or_ `pt_BR`** _— and_ **_remove the_ `#` _from the relevant lines_**_, as shown below:_

    #en_US.UTF-8 UTF-8
**_or_**

    #pt_BR.UTF-8 UTF-8
**_Final result:_**

    en_US.UTF-8 UTF-8
**_or_**

    pt_BR.UTF-8 UTF-8

_Then_ **_use CTRL+O to save, CTRL+X to exit, and run_ `locale-gen` _to generate the locales._**
> **Note: You can enable multiple languages, but you must choose one as the primary system language.**

_Now,_ **_apply the chosen primary language using_ `echo "LANG=[your language].UTF-8" > /etc/locale.conf`.** _In my case:_ `echo "LANG=en_US.UTF-8" > /etc/locale.conf`_ or _ `echo "LANG=pt_BR.UTF-8" > /etc/locale.conf`_._
> **Note: Ensure the language chosen here matches the one you uncommented in the locale.gen file.**

_Next, we will_ **_configure the keyboard layout and name the computer. To set the keyboard layout, use_ `echo "KEYMAP=[your layout]" > /etc/vconsole.conf`.** _For example:_ `echo "KEYMAP=br-abnt2" > /etc/vconsole.conf`_ or _ `echo "KEYMAP=us" > /etc/vconsole.conf`_._

_Now,_ **_to set the computer name, use_ `echo "[name]" > /etc/hostname`_,_** _for example:_ `echo "secura" > /etc/hostname`_._
> **Note: The `hostname` is how your computer will be identified on your network and in the terminal.**

**4 - Configure the Bootloader:** _To configure GRUB (installed in step 6.1), we will apply custom adjustments. **Use `nano /etc/default/grub` to open the file**; locate the third line and **change `GRUB_TIMEOUT=5` to `-1`**. This results in **`GRUB_TIMEOUT=-1`**, disabling the countdown so the system waits for your manual selection. **If you are setting up a dual-boot, go to the last line and uncomment `GRUB_DISABLE_OS_PROBER=false` by removing the `#`. Save with CTRL+O and exit with CTRL+X.**_

_Then,_ **_install GRUB to the partition with_ `grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB` _and generate the configuration file using_ `grub-mkconfig -o /boot/grub/grub.cfg`_._**

**5 - User Creation and Permissions:** _To secure the system, we must set access credentials. First,_ **_set a password for the administrator (`root`) using the_ `passwd` _command; the system will prompt you to enter and confirm the password_** _(it is highly recommended that this password be different from your personal user password)._

_Next,_ **_to create your user account, use the command_ `useradd -m -g wheel -s /bin/bash [your_user]`** _— e.g._ `useradd -m -g wheel -s /bin/bash guihnxz`_. Then,_ **_assign a password to this user with_ `passwd [your_user]`**_ (e.g.,_ `passwd guihnxz`_)_.

_Finally,_ **_run_ `EDITOR=nano visudo` _to open the `sudoers` file (ensure EDITOR is in caps). Use CTRL+W to find the `wheel` term and remove the `#` from the line_ `%wheel ALL=(ALL:ALL) ALL`_._** _This allows your user to use `sudo` for administrator privileges, requiring your user password for authentication._


## 7 - Activate Services and Finish the Installation <a name="final"></a>
**1 - Activate Services:** _To ensure the system functions correctly after rebooting, we must enable two essential services. First,_ **_enable Network Manager with_ `systemctl enable NetworkManager`**_ to manage Wi-Fi. Then,_ **_enable the Ly service_** _(installed in step 6.1)_ **_using_ `systemctl enable ly.service`_ so the login screen appears on startup instead of a bare terminal._**

# The End
## That's it! Your system is now usable and configured with the basics, ready for your personal customization. That wasn't so bad, was it? lol
