# My Install Guide for Arch Linux & Hyprland
## This is my personal guide to installing this setup.  
These are the paths we'll follow:
<details>
    <summary>1. Preparation</summary>

In this step, we'll prep the environment for **downloading the ISO**, **making your bootable USB**, **grabbing Ventoy** (the tool that'll **create the bootable USB**) and then **building that bootable USB**.
</details>

<details>
    <summary>2. BIOS/UEFI Setup</summary>

**We'll set the bootable USB to boot first.**
</details>

<details>
    <summary>3. Basic Live Environment Setup</summary>

Here we'll handle some **basic tweaks in the live env**. Stuff like **keyboard layout, internet, clock sync, and keyring fixes**.
</details>

<details>
    <summary>4. Disk Partitioning</summary>

In this step, we'll **sort out your disk** (HD or SSD). We'll **split the space, set the boot partition for startup, the swap partition** (extra RAM help when things get heavy), **and the root partition** for all files and the OS.
</details>

<details>
    <summary>5. Base Install</summary>

Here we'll **update mirrors and use `pacstrap` to drop in the Linux kernel** plus key packages. Then **generate the fstab with `genfstab` so it auto-mounts** the partitions you set up in step 4.
</details>

<details>
    <summary>6. Initial System Settings</summary>

We'll dive into **system internals**. Activate **multilib lib, set timezone, locale, language, hostname, grab and config the bootloader** (GRUB) for smooth boots, and **create a user with admin rights**.
</details> 

<details>
    <summary>7. Activate Services & Finish Install</summary>

Finally, **enable services for proper boot** and **unmount the live env then reboot**.
</details> 

***

## 📢 IMPORTANT NOTICE
### This guide is meant to be straightforward and easy to follow. But to nail the install without issues, stick to every instruction exactly—spaces, caps, and special chars matter.

***
# 🛠️ Step-by-Step Install
*Hit ALL sections below after reading the prep.*

## 1. Preparation {#preparation}
**1 - Grab the ISO: Head to the [official Arch Linux site](https://archlinux.org/download/) and snag the ISO.** It's the system "installer".

**2 - USB Backup: Back up EVERYTHING on your USB first. Next step wipes it clean.**

**3 - Download Ventoy: Grab it from [Ventoy's official site](https://www.ventoy.net/en/download.html).** It's the tool to prep your USB for booting Arch.

**4 - Build the Boot USB: Follow this [3-min tutorial](https://www.youtube.com/watch?v=3HVAM1M3fQU).**
> Done? Jump to step 2.

## 2. BIOS Config {#bios}
**1 - Enter BIOS/UEFI: Restart the PC and spam the setup key (usually F2 or Del)** till the menu pops up.

**2 - Boot Order: Find Boot, bump your USB to the top. Save, exit—PC reboots into Arch installer.**
> Done? On to step 3.

## 3. Basic Live Env Config {#live}
> **Pro tip: Valid commands often show no output. Linux only chats if there's an error. Empty prompt after? It worked.**

> Tiny font? Run `setfont ter-132b` to bump it up.

**1 - Keyboard Setup: First, set your layout so keys like / or - work right. Got Ç? That's br-abnt2 (BR). No? us.** Run `loadkeys [your layout]` like `loadkeys br-abnt2` or `loadkeys us`.

> Wired internet? Skip next.

**2 - WiFi: No cable? Fire up `iwctl` for IWD.** Prompt changes. **Spot your card with `device list` (wlan0-ish).** Then `station [card] scan`, `station [card] get-networks`, `station [card] connect [network]` + pass. `exit` to quit.

> **Ex: `station wlan0 scan`, `station wlan0 get-networks`, `station wlan0 connect wifi01`.**

> Test: `ping -c 3 google.com`. ms replies? Connected, skip DNS. No? Fix below.

<details>
    <summary>DNS Fix</summary>

**Edit resolv.conf with nano: `nano /etc/resolv.conf`. Wipe it, add `nameserver 1.1.1.1`, Enter, `nameserver 8.8.8.8`. CTRL+O (save+Enter), CTRL+X out.**
> Retest `ping -c 3 google.com`. ms? Good.
</details>

**3 - Clock Sync: `timedatectl set-ntp true` for global time sync.**

**4 - Keyring Fix: Nuke old: `rm -rf /etc/pacman.d/gnupg`. Init new: `pacman-key --init`, `pacman-key --populate archlinux`. Update: `pacman -Sy archlinux-keyring --noconfirm`.**
> Done? Step 4.

## 4. Disk Partitioning {#partitioning}
> **UEFI or BIOS? `ls /sys/firmware/efi/`. Folders? UEFI. Empty? BIOS (Legacy). Pick your path.**

<details>
    <summary>4.1 - UEFI</summary>

**1 - Find Disk: `lsblk` lists drives. HD/SATA SSD: sda/vda. NVMe: nvme0n1.**

**2 - Wipe Disk: `cfdisk /dev/[disk]` (ex: `cfdisk /dev/sda`). Pick gpt. Del all partitions till Free space only.**

**3 - Partition: New > 1G > Type > EFI System. Free space > New > [RAM size in GB]G > Type > Linux Swap. Free space > New > rest > Linux filesystem. Write > yes. Quit.**

**4 - Format: `mkfs.ext4 /dev/sda3` (root). `mkfs.fat -F32 /dev/sda1` (boot; skip if dualboot). `mkswap /dev/sda2`, `swapon /dev/sda2`.**

**5 - Mount: `mount /dev/sda3 /mnt`. `mkdir -p /mnt/boot/efi`. `mount /dev/sda1 /mnt/boot/efi`.**
</details>
<details>
    <summary>4.2 - BIOS</summary>

</details>

> Done? Step 5.

## 5. Base Install {#base-install}
**1 - Mirrors: `reflector --country [Your Country] --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`.**
> **Ex: `reflector --country Brazil --latest 20 --sort rate --verbose --save /etc/pacman.d/mirrorlist`**

**2 - Pacstrap: `pacstrap /mnt base linux linux-firmware base-devel`.**

**3 - Fstab: `genfstab -U /mnt >> /mnt/etc/fstab`.**

> Done? Step 6.

## 6. Initial System Settings {#iss}
> **Base installed. Chroot in: `arch-chroot /mnt`. Now we're inside.**

**1 - Essential Packages:**
- **NetworkManager:** Handles WiFi/Ethernet.
- **Sudo:** Lets users run root cmds.
- **Grub:** Bootloader.
- **efibootmgr:** Manages EFI boots.
- **os-prober:** Spots other OSes (for dualboot).
- **nano:** Terminal text editor.
- **kitty:** Fast, customizable terminal.
- **hyprland:** Wayland WM for your desktop.
- **xorg-xwayland:** X11 compat in Wayland.
- **xdg-desktop-portal-hyprland:** For screenshots/sharing.
- **mesa:** Open graphics drivers.
- **pipewire/pipewire-pulse:** Audio servers.
- **alsa-utils:** Sound tools.
- **Fonts (Fira Code, Comfortaa, Emoji):** Text/emojis.
- **ly:** Lightweight login screen.

> **Swap these if you want—my prefs here.**

**Cmd:** `pacman -Syu`. Then:  
`pacman -S networkmanager sudo grub efibootmgr os-prober nano kitty hyprland xorg-xwayland xdg-desktop-portal-hyprland mesa pipewire pipewire-pulse alsa-utils ttf-fira-code ttf-comfortaa noto-fonts-emoji ly --noconfirm`

**2 - Multilib: `nano /etc/pacman.conf`. CTRL+F "multilib", uncomment:**  
```
[multilib]
include = /etc/pacman.d/mirrorlist
```  
CTRL+O, Enter, CTRL+X.

**3 - Timezone/Keyboard/Hostname: `ln -sf /usr/share/zoneinfo/[Continent]/[City] /etc/localtime` (ex: America/Sao_Paulo). `hwclock --systohc`.**
> **Compound city? Use _ (São_Paulo).**

**Locale: `nano /etc/locale.gen`. CTRL+F lang (en_US or pt_BR), uncomment:**  
`en_US.UTF-8 UTF-8`  
CTRL+O, CTRL+X. `locale-gen`.  
`echo "LANG=en_US.UTF-8" > /etc/locale.conf` (match your pick).  

**Vconsole: `echo "KEYMAP=br-abnt2" > /etc/vconsole.conf` (or us).**  
**Hostname: `echo "secura" > /etc/hostname`.**

**4 - GRUB: `nano /etc/default/grub`. Set `GRUB_TIMEOUT=-1`. Dualboot? Uncomment `GRUB_DISABLE_OS_PROBER=false`. CTRL+O, CTRL+X.**  
`grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB`. `grub-mkconfig -o /boot/grub/grub.cfg`.

**5 - Root Pass: `passwd`.**  
**User: `useradd -m -g wheel -s /bin/bash guihnxz`. `passwd guihnxz`.**  
**Sudo: `EDITOR=nano visudo`. CTRL+F "%wheel", uncomment `%wheel ALL=(ALL:ALL) ALL`. CTRL+O, CTRL+X.**

## 7. Services & Finish {#final}
**1 - Services: `systemctl enable NetworkManager`. `systemctl enable ly.service`.**

# The End
## Done! System's ready to tweak. Not so bad, right? lol
