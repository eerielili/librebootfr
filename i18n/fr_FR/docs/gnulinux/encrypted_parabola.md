---
title: Installer Parabola ou Arch GNU+Linux-Libre, avec le chiffrement du disque tout entier (incluant /boot)
x-toc-enable: true
...

Consultez aussi:
[Installer Parabola ou Arch GNU+Linux-Libre, avec le chiffrement du disque tout entier (incluant /boot)](https://wiki.hyperbola.info/en:guide:encrypted_installation)

Ce guide couvre comment installer Parabola GNU+Linux-libre, avec le chiffrement
du disque tout entier, incluant **/boot** (le répertoire de démarrage). Sur la
majorité des systèmes, **/boot** doit être laissé non chiffré pendant que l'/les
autre(s) partition(s) est/sont chiffrée(s).
C'est ainsi que GRUB (et alors le kernel) peut être chargé et éxecuté,
parce que la majorité des micrologiciels ne peuvent pas ouvrir un 
volume LUKS; cependant, avec libreboot, GRUB est déjà inclus en tant que
[charge utile](http://www.coreboot.org/Payloads#GRUB_2), donc même **/boot**
peut être chiffré; ça protège **/boot** d'altérations par quelqu'un ayant
un accés physique à la machine.

**NOTE: Ce guide est *seulement* pour la charge utile GRUB.
Si vous utilisez la charge utile depthcharge, ignorez entièrement
cette section.**

Ce guide s'inspire massivement du wiki de Parabola, et le référencera constamment.
Pour les novices de Parabola GNU+Linux-Libre, jetez un coup d'oeil à leur
[section Débutants](https://wiki.parabola.nu/Beginners%27_guide#Beginners) pour une
vue d'ensemble.

## Configuration minimale requise
Vous pouvez trouver la configuration minimale requise
pour éxecuter et faire marcher Parabola GNU+Linux 
[sur le wiki de Parabola](https://wiki.parabola.nu/Beginners%27_guide#Minimum_system_requirements).

## Préparation

### Télécharger la dernière ISO
Pour ce guide j'ai utilisé l'ISO *2016.11.03*; l'image CD la plus
récente est disponible sur la [page des téléchargements](https://wiki.parabola.nu/Get_Parabola#Main_live_ISO://wiki.parabola.nu/Get_Parabola#Release_images_for_x86_64_and_i686_architectures) de Para-
-bola.

NdT: j'ai mis à jour les liens car le wiki a changé depuis.

Si vous êtes un débutant complet avec GNU+Linux, choisissez le
*LXDE Desktop ISO*. C'est plus facile d'installer Parabola avec
cette version parce qu'elle vous permet d'accéder un navigateur
web, comme ça vous pouvez copier-coller les commandes directement
dans le terminal, sans se soucier des fautes de frappe.

**NOTE: Vous ne devriez jamais copier-coller aveuglément n'importe
quelles commandes. Dans ce guide, le copie-collage permet de s'assurer
que vous ne faites pas d'erreurs quand vous entrez les commandes, et donc
que vous ne 'briquer' pas votre installation, et devoir tout recommencer.
C'est important de savoir ce que chaque commande fait avant que l'utilisiez,
soyez donc sûr pour chacune d'entre elles de lire la documentation du wiki
Parabola/Arch sur celles-ci, ainsi que sa page** `man` **.**

Si vous n'êtes pas un débutant, choisissez le *Main Live ISO*

Choisissez seulement le *TalkingParabola ISO*, si vous êtes aveugle
ou malvoyant.

### Choisir le périphérique d'installation
Référez vous au wiki Parabolab pour trouver le bon périphérique d'installation,
que vous utilisiez soit un [disque optique](https://wiki.parabola.nu/Beginners%27_guide#Optical_Disks)
ou une [clef USB](https://wiki.parabola.nu/Beginners%27_guide#USB_flash_drive).

## Démarrer l'environnement d'installation de Parabola
Après avoir téléchargé l'ISO et créé une sorte de périphérique démarrable,
vous aurez besoin de démarrer dans l'image Live. Si vous n'êtes pas sûr de savoir
comment faire, voyez [Comment démarrer un installeur GNU+Linux](grub_boot_installer.md) 
puis passez à l'étape suivante; sinon, passez juste à l'étape suivante.

Une fois que vous avez démarré dans l'environnement, ouvrez soit le **`Terminal LXDE`**
(si vous utilisez l'ISO Bureau LXDE), ou juste rentrez simplement les commandes listées
ci-dessous (si vous utilisez n'importe quel autre ISO).

## Configurer la disposition du clavier
Pour bien commencer l'installation, vous devez d'abord choisir
la bonne [disposition de votre clavier](https://wiki.parabola.nu/Beginners%27_guide#Changing_Keyboard).

## Établir une connexion Internet
Vous aurez aussi besoin [de configurer une connexion réseau](https://wiki.parabola.nu/Beginners%27_guide#Establish_an_internet_connection) 
pour installer des paquets.

## Préparer le périphérique de stockage pour l'installation

Vous allez avoir besoin de préparer le périphérique de stockage que nous utiliserons
pour installer le système d'exploitation.
Vous pouvez utiliser la même méthode pour trouver le [nom du périphérique](https://wiki.parabola.nu/Beginners%27_guide#USB_flash_drive)
afin de déterminer le périphérique d'installation pour l'ISO.

### Wipe Storage Device
You want to make sure that the device you're using doesn't contain any plaintext
copies of your personal data. If the drive is new, then you can skip the rest of this section;
if it's not new, then there are two ways to handle it:

1. If the drive were not previously encrypted, securely wipe it with the `dd` command;
you can either choose to fill it with zeroes or random data; I chose random data (e.g., `urandom`),
because it's more secure. Depending on the size of the drive, this could take a while to complete:

    ~~~
    # dd if=/dev/urandom of=/dev/sdX; sync
    ~~~

2. If the drive were previously encrypted, all you need to do is wipe the LUKS header.
The size of the header depends upon the specific model of the hard drive;
you can find this information by doing some research online.
Refer to this [article](https://www.lisenet.com/2013/luks-add-keys-backup-and-restore-volume-header/), for more information about LUKS headers.
You can either fill the header with zeroes, or with random data; again, I chose random data, using `urandom`:

    ~~~
    # head -c 3145728 /dev/urandom > /dev/sdX; sync
    ~~~

Also, if you're using an SSD, there are a two things you should keep in mind:

-    There are issues with TRIM; it's not enabled by default through LUKS,
and there are security issues, if you do enable it. See [this page](https://wiki.archlinux.org/index.php/Dm-crypt#Specialties) for more info.
-    Make sure to read [this article](https://wiki.archlinux.org/index.php/Solid_State_Drives),
for information on managing SSD's in Arch Linux (the information applies to Parabola, as well).

### Formatting the Storage Device
Now that all the personal data has been deleted from the disk, it's time to format it.
We'll begin by creating a single, large partition on it, and then encrypting it using LUKS.

#### Create the LUKS partition
You will need the `device-mapper` kernel module during the installation;
this will enable us to set up our encrypted disk. To load it, use the following command:

    # modprobe dm_mod

We then need to select the **device name** of the drive we're installing the operating system on;
see the above method, if needed, for figuring out device names.

Now that we have the name of the correct device, we need to create the partition on it.
For this, we will use the `cfdisk` command:

    # cfdisk /dev/sdX

1. Use the arrow keys to select your partition, and if there is already a partition
on the drive, select **Delete**, and then **New**.
2. For the partition size, leave it as the default, which will be the entire drive.
3. You will see an option for **Primary** or **Logical**; choose **Primary**,
and make sure that the partition type is **Linux (83)**.
4. Select **Write**; it will ask you if you are sure that you want to overwrite the drive.
5. Type **yes**, and press enter. A message at the bottom will appear, telling you that
the partition table has been altered.
6. Select **Quit**, to return you to the main terminal.

Now that you have created the partition, it's time to create the encrypted volume on it,
using the `cryptsetup` command, like this:

    # cryptsetup -v --cipher serpent-xts-plain64 --key-size 512 --hash whirlpool \
    --iter-time 500 --use-random --verify-passphrase --type luks1 luksFormat /dev/sdXY

These are just recommended defaults; if you want to juse anything else,
or to find out what options there are, run `man cryptsetup`.

>**NOTE: the default iteration time is 2000ms (2 seconds),
>if not specified when running the cryptsetup command. You should set a lower time than this;
>otherwise, there will be an approximately 20-second delay when booting your
>system. We recommend 500ms (0.5 seconds), and this is included in the
>prepared** `cryptsetup` **command above. Keep in mind that the iteration time
>is for security purposes (it mitigates brute force attacks), so anything lower
>than 0.5 seconds is probably not very secure.**

You will now be prompted to enter a passphrase; be sure to make it *secure*.
For passphrase security, length is more important than complexity
(e.g., **correct-horse-battery-staple** is more secure than **bf20$3Jhy3**), 
but it's helpful to include several different types of characters 
(e.g., uppercase/lowercase letters, numbers, special characters).
The password length should be as long as you are able to remember,
without having to write it down, or store it anywhere.

Use of the [**diceware**](http://world.std.com/~reinhold/diceware.html) method
is recommended, for generating secure passphrases (rather than passwords).

#### Create the Volume Group and Logical Volumes
The next step is to create two Logical Volumes within the LUKS-encrypted partition:
one will contain your main installation, and the other will contain your swap space.

We will create this using, the [Logical Volume Manager (LVM)](https://wiki.archlinux.org/index.php/LVM).

First, we need to open the LUKS partition, at **/dev/mapper/lvm**:

    # cryptsetup luksOpen /dev/sdXY lvm

Then, we create LVM partition:

    # pvcreate /dev/mapper/lvm

Check to make sure tha the partition was created:

    # pvdisplay

Next, we create the volume group, inside of which the logical volumes will
be created. In libreboot's case, we will call this group **matrix**.
If you want to have it work via *Load Operating System (incl. fully
encrypted disks)  [o]* it needs to be called **matrix** (as it is harcoded
in libreboot's grub.cfg on the flash)

    # vgcreate matrix /dev/mapper/lvm

Check to make sure that the group was created:

    # vgdisplay

Lastly, we need to create the logical volumes themselves, inside the volume group;
one will be our swap, cleverly named **swapvol**, and the other will be our root partition,
equally cleverly named as **rootvol**.

1. We will create the **swapvol** first (again, choose your own name, if you like).
Also, make sure to [choose an appropriate swap size](http://www.linux.com/news/software/applications/8208-all-about-linux-swap-space)
(e.g., **2G** refers to two gigabytes; change this however you see fit):

    ~~~
    # lvcreate -L 2G matrix -n swapvol
    ~~~

2. Now, we will create a single, large partition in the rest of the space, for **rootvol**:

    ~~~
    # lvcreate -l +100%FREE matrix -n rootvol
    ~~~

You can also be flexible here, for example you can specify a **/boot**, a **/**,
a **/home**, a **/var**, or a **/usr** volume. For example, if you will be running a
web/mail server then you want **/var** (where logs are stored) in its own partition,
so that if it fills up with logs, it won't crash your system.
For a home/laptop system (typical use case), just a root and a swap will do.

Verify that the logical volumes were created correctly:

    # lvdisplay

#### Make the rootvol and swapvol Partitions Ready for Installation
The last steps of setting up the drive for installation are turning **swapvol**
into an active swap partition, and formatting **rootvol**.

To make **swapvol** into a swap partition, we run the `mkswap` (i.e., make swap) command:

    # mkswap /dev/mapper/matrix-swapvol

Activate the **swapvol**, allowing it to now be used as swap,
using `swapon` (i.e., turn swap on) command:

    # swapon /dev/matrix/swapvol

Now I have to format **rootvol**, to make it ready for installation;
I do this with the `mkfs` (i.e., make file system) command.
I choose the **ext4** filesystem, but you could use a different one,
depending on your use case:

    # mkfs.ext4 /dev/mapper/matrix-rootvol

Lastly, I need to mount **rootvol**. Fortunately, GNU+Linux has a directory
for this very purpose: **/mnt**:

    # mount /dev/matrix/rootvol /mnt

#### Separate boot and home logical volumes
You could also create two separate logical volumes for **/boot** and **/home**,
but such a setup would be for advanced users,
and is thus not covered in this guide.
If separate boot logical volume is used, it has to be named **boot**
in order for libreboot to use it.

The setup of the drive and partitions is now complete; it's time to actually install Parabola.

## Select a Mirror
The first step of the actual installation is to choose the server from where
we will need to download the packages; for this, we will again refer to the [Parabola Wiki](https://wiki.parabola.nu/Beginners%27_guide#Select_a_mirror).
For beginners, I recommend that the edit the file using `nano` (a command-line text editor);
you can learn more about it on [their website](https://www.nano-editor.org/); for non-beginners,
simply edit it with your favorite text editor.

## Install the Base System
We need to install the essential applications needed for your Parabola installation to run;
refer to [Install the Base System](https://wiki.parabola.nu/Beginners%27_guide#Install_the_base_system), on the Parabola wiki.

## Generate an fstab
The next step in the process is to generate a file known as an **fstab**;
the purpose of this file is for the operating system to identify the storage device
used by your installation. [On the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Generate_an_fstab) are the instructions to generate that file.

## Chroot into and Configure the System
Now, you need to `chroot` into your new installation, to complete the setup
and installation process. **Chrooting** refers to changing the root directory
of an operating system  to a different one; in this instance, it means changing your root
directory to the one you created in the previous steps, so that you can modify files
and install software onto it, as if it were the host operating system.

To `chroot` into your installation, follow the instructions [on the
Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Chroot_and_configure_the_base_system).

### Setting up the Locale
Locale refers to the language that your operating system will use, as well as some
other considerations related to the region in which you live. To set this up,
follow the instructions [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Locale).

### Setting up the Consolefont and Keymap
This will determine the keyboard layout of your new installation; follow the instructions [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Console_font_and_keymap).

### Setting up the Time Zone
You'll need to set your current time zone in the operating system; this will enable applications
that require accurate time to work properly (e.g., the web browser).
To do this, follow the instructions [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Time_zone).

### Setting up the Hardware Clock
To make sure that your computer has the right time, you'll have to set the time in your computer's internal clock.
Follow the instructions [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Hardware_clock) to do that.

### Setting up the Kernel Modules
Now we need to make sure that the kernel has all the modules that it needs
to boot the operating system. To do this, we need to edit a file called **mkinitcpio.conf**.
More information about this file can be found [in the Parabola beginner's guide](https://wiki.parabola.nu/Mkinitcpio),
but for the sake of this guide, you simply need to run the following command.

    # nano /etc/mkinitcpio.conf

There are several modifications that we need to make to the file:

1.  Change the value of the uncommented `MODULES` line to `i915`.

    * This forces the driver to load earlier, so that the console font you selected earlier
      isn’t wiped out after getting to login.
    * If you are using a **Macbook 2,1** you will also need to add `hid-generic`,
      `hid`, and `hid-apple` inside the quotation marks, in order to have
      a working keyboard when asked to enter the LUKS password.
      Make sure to separate each module by one space.

2.  Change the value of the uncommented `HOOKS` line to the following:

    ~~~
    base udev autodetect modconf block keyboard keymap consolefont encrypt lvm2 filesystems fsck shutdown
    ~~~

    here's what each module does:

    * `keymap` adds to *initramfs* the keymap that you specified in **/etc/vconsole.conf**
    * `consolefont` adds to *initramfs* the font that you specified in **/etc/vconsole.conf**
    * `encrypt` adds LUKS support to the initramfs - needed to unlock your disks at boot time
    * `lvm2` adds LVM support to the initramfs - needed to mount the LVM partitions at boot time
    * `shutdown` is needed according to Parabola wiki, for unmounting devices (such as LUKS/LVM) during shutdown

After modifying the file and saving it, we need to update the kernel(s) with the new settings.
Before doing this, we want to install a Long-Term Support (LTS) kernel as a backup, in the event
that we encounter problems with the default Linux-Libre kernel (which is continually updated).

We will also install the `grub` package, which we will need later,
to make our modifications to the GRUB configuration file:

    # pacman -S linux-libre-lts grub

Then, we update both kernels like this, using the `mkinitcpio` command:

    # mkinitcpio -p linux-libre
    # mkinitcpio -p linux-libre-lts

### Setting up the Hostname
Now we need to set up the hostname for the system; this is so that our device
can be identified by the network. Refer to [the hostname section](https://wiki.parabola.nu/Beginners%27_guide#Hostname)
of the Parabola wiki's Beginner's Guide. You can make the hostname anything you like;
for example, if you wanted to choose the hostname **parabola**,
you would run the `echo` command, like this:

    # echo parabola > /etc/hostname

And then you would modify **/etc/hosts** like this, adding the hostname to it:

    # nano /etc/hosts

    #<ip-address> <hostname.domain.org>   <hostname>
    127.0.0.1     localhost.localdomain   localhost   parabola
    ::1           localhost.localdomain   localhost   parabola

### Configure the Network
Now that we have a hostname, we need to configure the settings for the rest of the network.
Instructions for setting up a wired connection are [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Wired),
and instructions for setting up a wireless connection are [in the Parabola beginner's guide](https://wiki.parabola.nu/Beginners%27_guide#Wireless_2).

### Set the root Password
The **root** account has control over all the files in the computer; for security,
we want to protect it with a password. The password requirements given above,
for the LUKS passphrase, apply here as well. You will set this password with the `passwd` command:

    # passwd

### Extra Security Tweaks
There are some final changes that we can make to the installation, to make it
significantly more secure; these are based on the [Security](https://wiki.archlinux.org/index.php/Security) section of the Arch wiki.

#### Key Strengthening
We will want to open the configuration file for password settings, and increase
the strength of our **root** password:

    # nano /etc/pam.d/passwd

Add `rounds=65536` at the end of the uncommented 'password' line; in simple terms,
this will force an attacker to take more time with each password guess, mitigating
the threat of brute force attacks.

#### Restrict Access to Important Directories
You can prevent any user, other than the root user, from accessing the most important
directories in the system, using the `chmod` command; to learn more about this command,
run `man chmod`:

    # chmod 700 /boot /etc/{iptables,arptables}

#### Lockout User After Three Failed Login Attempts
We can also setup the system to lock a user's account, after three failed login attempts.

To do this, we will need to edit the file **/etc/pam.d/system-login**,
and comment out this line:

    auth required pam\_tally.so onerr=succeed file=/var/log/faillog*\

You could also just delete it. Above it, put the following line:

    auth required pam\_tally.so deny=2 unlock\_time=600 onerr=succeed file=/var/log/faillog

This configuration will lock the user out for ten minutes.
You can unlock a user's account manually, using the **root** account, with this command:

    # pam_tally --user *theusername* --reset
    
#### Generate grub.cfg
Edit configuration in `/etc/default/grub`, remembering to use UUID when poitning to mbr/gpt partition.
Use `blkid` to get list of devices with their respective UUIDs.
For details see [parabola wiki.](https://wiki.parabola.nu/Dm-crypt/Encrypting_an_entire_system#Configuring_the_boot_loader_5)

Next generate grub.cfg with:

    # grub-mkconfig -o /boot/grub/grub.cfg
    
If you have separate `/boot` partition, don't forget to add `boot` symlink inside that points to current directory

    # cd /boot; ln -s . boot

## Unmount All Partitions and Reboot
Congratulations! You have finished the installation of Parabola GNU+Linux-Libre.
Now it is time to reboot the system, but first, there are several preliminary steps:

Exit from `chroot`, using the `exit` command:

    # exit

Unmount all of the partitions from **/mnt**, and "turn off" the swap volume:

    # umount -R /mnt
    # swapoff -a

Deactivate the **rootvol** and **swapvol** logical volumes:

    # lvchange -an /dev/matrix/rootvol
    # lvchange -an /dev/matrix/swapvol

Lock the encrypted partition (i.e., close it):

    # cryptsetup luksClose lvm

Shutdown the machine:

    # shutdown -h now

After the machine is off, remove the installation media, and turn it on.

## Booting the installation manually from GRUB
When you forget to configure or misconfigure grub on your hdd, you have to manually boot
the system by entering a series of commands into the GRUB command line.


After the computer starts, Press `C` to bring up the GRUB command line.
You can either boot the normal kernel, or the LTS kernel we installed;
here are the commands for the normal kernel:

    grub> cryptomount -a
    grub> set root='lvm/matrix-rootvol'
    grub> linux /boot/vmlinuz-linux-libre root=/dev/matrix/rootvol cryptdevice=/dev/sda1:root
    grub> initrd /boot/initramfs-linux-libre.img
    grub> boot

If you're trying to boot the LTS kernel, simply add **-lts** to the end
of each command that contains the kernel (e.g., **/boot/vmlinuz-linux-libre**
would be **/boot/vmlinuz/linux-libre-lts**).

**NOTE: on machines with native sata, during boot a (faulty) optical disc drive (like dvd) can cause
the** `cryptomount -a` **command to fail/hang, as well as the error** `AHCI transfer timed out`
**The workaround was to remove the DVD drive.**

## Follow-Up Tutorial: Configuring Parabola
The next step of the setup process is to modify the configuration file that
GRUB uses, so that we don't have to manually type in those commands above, each time we want
to boot our system.

To make this process much easier, we need to install a graphical interface,
as well as install some other packages that will make the system more user-friendly.
These additions will also sharply reduce the probability of "bricking" our computer.

[Configuring Parabola (Post-Install)](configuring_parabola.md) provides an example setup, but don't feel
as if you must follow it verbatim (of course, you can, if you want to);
Parabola is user-centric and very customizable, which means that you have maximum control
of the system, and a near-limitless number of options for setting it up. For more information,
read [The Arch Way](https://wiki.archlinux.org/index.php/The_Arch_Way) (Parabola also follows it).

After setting up the graphical interface, refer to [How to Modify GRUB Configuration](grub_cbfs.md),
for instructions on doing just that, as well as flashing the ROM (if necessary).

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>

Copyright © 2015 Jeroen Quint <jezza@diplomail.ch>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)

