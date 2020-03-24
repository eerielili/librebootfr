---
title: Installer Debian ou Devuan GNU+Linux avec chiffrement du disque en entier (incluant /boot)
...

Ce guide est écrit pour la distribution Debian, mais ça devrait aussi
marcher pour Devuan avec le netinstallateur.

Carte mère Gigabyte GA-G41M-ES2L
=====================

Pour démarrer le netinstallateur de Trisquel, assurez-vous de rajouter fb=false
aux paramètres du kernel dans GRUB. Ça démarrera l'installeur en mode texte au
lieu d'utiliser un tampon d'image.

C'est parti...
============
Libreboot sur l'architecture x86 utilise la
[charge utile GRUB](http://www.coreboot.org/Payloads#GRUB_2) par défaut, ce
qui veut dire que le fichier de configuration GRUB (d'où votre menu GRUB vient)
est stocké directement aux côtés de libreboot et de sa charge utile GRUB
éxecutable, à l'intérieur de la puce de flash. 
Dans le contexte, ça veut dire qu'installer des distributions et les manager
est pris en charge d'une manière légèrement différente comparé aux 
systèmes BIOS traditionnels.

Sur la majorité des systèmes, la partition /boot doit être laissée non chiffrée
pendant que les autres le sont. C'est pour celà que GRUB, et donc le kernel, peut
être chargé et éxecuté puisque le micrologiciel ne peut pas ouvrir un volume
LUKS. Il n'en est pas ainsi avec libreboot ! Car GRUB est déjà inclus directement
en tant que charge utile, même /boot peut être chiffré. Ça protège /boot d'altérations
venant d'une personne ayant un accés physique à la machine.

Ce guide est écrit pour le netinstallateur Debian. Vous pouvez télécharger
l'ISO depuis la page d'accueil sur [debian.org](https://www.debian.org). Utilisez
ceci dans le terminal GRUB pour le démarrer depuis une clef USB (pour architecture
Intel 64-bit ou AMD):

    set root='usb0'
    linux /install.amd/vmlinuz
    initrd /install.amd/initrd.gz
    boot

Si vous êtes sur un système 32-bit (p.e. X60):

     set root='usb0'
     linux /install.386/vmlinuz
     initrd /install.i386/initrd.gz
     boot

[Ce guide](grub_boot_installer.md) montre comment créer une clef USB avec l'image
ISO de Debian.

*Ce guide est seulement pour la charge utile GRUB. Si vous utilisez la charge utile
deptcharge, ignorez entièrement cette section*.

Note: sur quelque ThinkPads, un lecteur DVD défectueux peut causer l'échec de l'étape
cryptomount -a durant le démarrage. Si ça vous arrive, essayez d'enlever le lecteur.


    set root='usb0'
    linux /install.386/vmlinuz
    initrd /install.386/initrd.gz
    boot

Définissez un mot de passe utilisateur robuste (beaucoups de lettres minuscules/majuscules,
nombres et symboles). L'utilisation de la *méthode lancer de dés* est recommandée, pour
générer des phrases de passe sécurisées (au lieu de mots de passes).

Quand l'installeur vous demande de mettre en place le chiffrement (ecryptfs)
pour votre répertoire maison/personnel, choisissez 'Oui' si vous voulez : *LUKS
est déjà sécurisé et marche bien. Avoir ecryptfs au-dessus ajoutera une pénalite
remarquable aux perfomances pour un gain minime en sécurité dans la majorité des
cas d'utilisation. C'est par conséquent optionnel et non recommandé. Choisissez
'Non'.*

*Votre mot de passe utilisateur devrait être différent du mot de passe LUKS que 
vous mettrez en place pls tard. Votre mot de passe LUKS devrait être comme le
mot de passe utilisateur, c'est à dire sécurisé.*

Partitionnage
============

Choisissez le partitionnage 'Manuel':

-    Sélectionnez le disque et créez une nouvelle table de partition
-    Une seule large partition. Ce qui suit sont pour la plupart configurés par défaut:
    -    Utiliser: en tant que volume physique pour le chiffrement
    -    Chiffrement: aes
    -    taille de clé: le paramètre par défaut donné
    -    algorithme IV: le paramètre par défaut donné
    -    Clé de chiffrement: phrase de passe
    -    Effacer les données: Oui (choisissez 'Non' seulement si c'est un nouveau
     disque qui ne contient pas vos données personnelles.)

-    Sélectionnez 'configurer des volumes chiffrés'
    -    Créer des volumes chiffrés
    -    Sélectionnez votre partition
    -    Finir
    -    Vraiment effacer: Oui
    -    (l'effaçage prend un certain temps. Soyez patient)
    -    (si votre système précédent était chiffré, laissez tourner pour
          une minute environ afin de s'assurer que l'en-tête LUKS est sorti)
-    Sélectionner l'espace chiffré:
    -    utiliser en tant que: volume physique pour LVM
    -    Choisir 'Terminer le paramétrage de la partition'
-    Configurer le manager des volumes logiques:
    -    Garder les paramètres: Oui
-    Créer un groupe de volumes:
    -  Nom: `matrix` (utilisez ce nom à la lettre)
    -  Sélectionner partition crypto
-   Créer un volume logique
    -   sélectionnez `matrix` (utilisez ce nom à la lettre)
    -   nom: `rootvol` (utilisez ce nom à la lettre)
    -   taille: défaut moins 2048Mo
-   Créer un volume logique
    -   sélectionnez `matrix` (utilisez ce nom à la lettre)
    -   nom: `swap` (utilisez ce nom à la lettre)
    -   taille: pressez Entrée

Partitionnage avancé
====================
Maintenant vous etes de retour sur l'écran de partitionnage
principal. Vous allez simplement définir les points de montage et
systèmes de fichier à utiliser.

-   LVM VL rootvol
    -   utiliser en tant que : btrfs
    -   point de montage: /
    -   finir le paramétrage de cette partition
-   LVM VL swap
    -   utiliser en tant que: zone de swap
    -   finir le paramétrage de cette partition
-   Maintenant sélectionnez 'A fini le partitionnage et écrire les changements 
    sur disque'.

Kernel
======

L'installation demandera quel kernel vous voulez utiliser. linux-generic 
fait l'affaire.


Tasksel
=======

Pour Debian, utilisez l'option *MATE* ou une des autres si vous le 
voulez. Le projet libreboot recommande MATE, à moins que vous êtes
assez expérimenté pour choisir quelque chose d'autre.

Si vous voulez debian-testing

Tasksel
=======

For Debian, use the *MATE* option, or one of the others if you want. The
libreboot project recommends MATE, unless you're saavy enough to choose
something else.

If you want debian-testing, then you should only select barebones
options here and change the entries in /etc/apt/sources.list after
install to point to the new distro, and then run `apt-get update` and
`apt-get dist-upgrade` as root, then reboot and run `tasksel` as
root. This is to avoid downloading large packages twice.

NOTE: If you want the latest up to date version of the Linux kernel,
Debian's kernel is sometimes outdated, even in the testing distro. You
might consider using [this repository](https://jxself.org/linux-libre/)
instead, which contains the most up to date versions of the Linux
kernel. These kernels are also deblobbed, like Debian's kernels, so you
can be sure that no binary blobs are present.

Postfix configuration
=====================

If asked, choose *"No Configuration"* here (or maybe you want to
select something else. It's up to you.)

Install the GRUB boot loader to the master boot record
======================================================

Choose 'Yes'. It will fail, but don't worry. Then at the main menu,
choose 'Continue without a bootloader'. You could also choose 'No'.
Choice is irrelevant here.

*Don't forget to have grub-coreboot package installed, even though installing grub to MBR is irrelevant
on libreboot system, grub tools are still needed to eg. generate config (`grub-mkconfig`)*

Clock UTC
=========

Just say 'Yes'.

Booting your system
===================

At this point, you will have finished the installation. At your GRUB
payload, press C to get to the command line, and enter:

    grub> cryptomount -a
    grub> set root='lvm/matrix-rootvol'
    grub> linux /vmlinuz root=/dev/mapper/matrix-rootvol cryptdevice=/dev/mapper/matrix-rootvol:root
    grub> initrd /initrd.img
    grub> boot

ecryptfs
========

If you didn't encrypt your home directory, then you can safely ignore
this section.

Immediately after logging in, do that:

    $ sudo ecryptfs-unwrap-passphrase

This will be needed in the future if you ever need to recover your home
directory from another system, so write it down and keep the note
somewhere secret. Ideally, you should memorize it and then burn the note
(or not even write it down, and memorize it still)>

Modify grub.cfg (CBFS)
======================

Now you need to set it up so that the system will automatically boot,
without having to type a bunch of commands.

Modify your grub.cfg (in the firmware) [using this
tutorial](grub_cbfs.md); just change the default menu entry 'Load
Operating System' to say this inside:

    cryptomount -a
    set root='lvm/matrix-rootvol'
    linux /vmlinuz root=/dev/mapper/matrix-rootvol cryptdevice=/dev/mapper/matrix-rootvol:root
    initrd /initrd.img
    
Without specifying a device, the *-a* parameter tries to unlock all
detected LUKS volumes. You can also specify -u UUID or -a (device).

[Refer to this guide](grub_hardening.md) for further guidance on
hardening your GRUB configuration, for security purposes.

Flash the modified ROM using [this tutorial](../install/#flashrom).

Troubleshooting
===============

A user reported issues when booting with a docking station attached on
an X200, when decrypting the disk in GRUB. The error *AHCI transfer
timed out* was observed. The workaround was to remove the docking
station.

Further investigation revealed that it was the DVD drive causing
problems. Removing that worked around the issue.

    "sudo wodim -prcap" shows information about the drive:
    Device was not specified. Trying to find an appropriate drive...
    Detected CD-R drive: /dev/sr0
    Using /dev/cdrom of unknown capabilities
    Device type    : Removable CD-ROM
    Version        : 5
    Response Format: 2
    Capabilities   : 
    Vendor_info    : 'HL-DT-ST'
    Identification : 'DVDRAM GU10N    '
    Revision       : 'MX05'
    Device seems to be: Generic mmc2 DVD-R/DVD-RW.

    Drive capabilities, per MMC-3 page 2A:

      Does read CD-R media
      Does write CD-R media
      Does read CD-RW media
      Does write CD-RW media
      Does read DVD-ROM media
      Does read DVD-R media
      Does write DVD-R media
      Does read DVD-RAM media
      Does write DVD-RAM media
      Does support test writing

      Does read Mode 2 Form 1 blocks
      Does read Mode 2 Form 2 blocks
      Does read digital audio blocks
      Does restart non-streamed digital audio reads accurately
      Does support Buffer-Underrun-Free recording
      Does read multi-session CDs
      Does read fixed-packet CD media using Method 2
      Does not read CD bar code
      Does not read R-W subcode information
      Does read raw P-W subcode data from lead in
      Does return CD media catalog number
      Does return CD ISRC information
      Does support C2 error pointers
      Does not deliver composite A/V data

      Does play audio CDs
      Number of volume control levels: 256
      Does support individual volume control setting for each channel
      Does support independent mute setting for each channel
      Does not support digital output on port 1
      Does not support digital output on port 2

      Loading mechanism type: tray
      Does support ejection of CD via START/STOP command
      Does not lock media on power up via prevent jumper
      Does allow media to be locked in the drive via PREVENT/ALLOW command
      Is not currently in a media-locked state
      Does not support changing side of disk
      Does not have load-empty-slot-in-changer feature
      Does not support Individual Disk Present feature

      Maximum read  speed:  4234 kB/s (CD  24x, DVD  3x)
      Current read  speed:  4234 kB/s (CD  24x, DVD  3x)
      Maximum write speed:  4234 kB/s (CD  24x, DVD  3x)
      Current write speed:  4234 kB/s (CD  24x, DVD  3x)
      Rotational control selected: CLV/PCAV
      Buffer size in KB: 1024
      Copy management revision supported: 1
      Number of supported write speeds: 4
      Write speed # 0:  4234 kB/s CLV/PCAV (CD  24x, DVD  3x)
      Write speed # 1:  2822 kB/s CLV/PCAV (CD  16x, DVD  2x)
      Write speed # 2:  1764 kB/s CLV/PCAV (CD  10x, DVD  1x)
      Write speed # 3:   706 kB/s CLV/PCAV (CD   4x, DVD  0x)

    Supported CD-RW media types according to MMC-4 feature 0x37:
      Does write multi speed       CD-RW media
      Does write high  speed       CD-RW media
      Does write ultra high speed  CD-RW media
      Does not write ultra high speed+ CD-RW media

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>\

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License Version 1.3 or any later
version published by the Free Software Foundation
with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts.
A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
