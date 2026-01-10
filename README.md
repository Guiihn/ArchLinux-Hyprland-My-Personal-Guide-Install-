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

<h2><b> 📢 IMPORTANT NOTICE </h2></b>
<h3><i>This tutorial is designed to be easy to understand and execute. However, to ensure successful installation and avoid system failures, it is essential that all instructions are strictly followed. Make sure you type the commands exactly as described, respecting spaces, capital letters, and special characters.</i></h3>

---
# 🛠️ Step-by-Step Installation
*Follow ALL sections below after completing the previous reading.*
