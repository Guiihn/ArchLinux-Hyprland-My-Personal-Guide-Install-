# **My Install Guide for Arch Linux & Hyprland**
### *This is my personal guide to installing this system.*
### *These will be the paths we will follow:*
<details>
    <summary><b>1. Preparation </b></summary>

In this step, we will focus on preparing the environment for **downloading the ISO**, **formatting the USB stick**, **downloading Ventoy** (the program that will be **used to create the bootable USB stick**) and then **creating the bootable USB stick**.
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
*Siga TODAS as seções abaixo após concluir a leitura anterior.*

## 1. Preparation <a name="preparation"></a>

1.1 - Download the official iso from the [Arch website](https://archlinux.org/download/)
