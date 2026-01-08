# **My Install Guide for Arch Linux & Hyprland**
### *This is my personal guide to installing this system.*
### *These will be the paths we will follow:*
<details>
    <summary><b>1. Preparation </b></summary>

In this step, we will focus on preparing the environment for **downloading the ISO**, **prepare your bootable USB stick**, **downloading Ventoy** (the program that will be **used to create the bootable USB stick**) and then **creating the bootable USB stick**.
</details>

<details>
    <summary><b>2. BIOS/UEFI Setup </b></summary>

**We will put the bootable USB stick to start first.**
</details>

<details>
    <summary><b>3. Basic configuration for Live Environment </b></summary>

In this stage we will do some **basic configurations in the Live environment**. These settings will be to **configure keyboard layout, internet, environment clock and keyring correction.**
</details>

<details>
    <summary><b>4. Disk Partitioning </b></summary>

In this step, the focus will be on **organizing your disk** (HD or SSD). Let's **decide how the space will be divided, defining the boot partition for the system to start, the swap partition** (auxiliary memory) to help the computer when it is overloaded, **and the main partition** where all the files and the operating system will be stored.
</details>

<details>
    <summary><b>5. Installation of the base </b></summary>

Here we will **update the mirrors and also use `pacstrap` to install the Linux Kernel** and essential packages. Finally, let's **generate the fstab file with `genfstab` to remember and automatically mount the partitions** that you had previously mounted in step 4.
</details>

<details>
    <summary><b>6. Initial system settings </b></summary>

In this step, we will focus on the **internal configuration of the system**. We will **activate an important library called multilib, configure the time zone, location and language, download and configure the bootloader** (GRUB) so that the system starts correctly, and finally, we will **perform user creation with administrator privileges**.
</details> 

---
# 🛠️ Step-by-Step Installation
*Follow ALL sections below after completing the previous reading.*

## 1. Preparation <a name="preparation"></a>
**1.1 - Download the ISO: _Go to the_ [official Arch Linux website](https://archlinux.org/download/) _and download the installation file (ISO)_**. _He is the "installer" of the system._

**1.2 - Pendrive Backup: _Save ALL your USB stick files elsewhere. The next step will erase everything in it._**

**1.3 - Download Ventoy: _Download the program from_ [Ventoy's official website](https://www.ventoy.net/en/download.html).** _It is the tool that prepares the USB stick so that the computer can start the Arch Linux installer._

**1.4 - Create the Installation Pendrive: _Just follow this_ [3 minute tutorial](https://www.youtube.com/watch?v=3HVAM1M3fQU) _to create the bootable USB stick._**
> If you have finished these stages, go to step 2.

## 2. BIOS Configuration <a name="bios"></a>
**2.1 - Access the BIOS/UEFI:** _Now, let's **restart the computer. Once it starts turning on, keep pressing the setup key (usually F2 or Delete)** until it opens a different screen._

**2.2 - Boot Order:** _Within that screen, **look for the Boot option and change the order so that your USB stick is first so that it is read first. Save the changes and exit;** the computer will restart and load the Arch Linux installer._ 
> If you have finished these stages, go to step 2.
