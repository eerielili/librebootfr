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

Si vous voulez debian-testing, alors vous devriez sélectionner le
minimum d'options possibles et changer les lignes dans /etc/apt/sources.list
après l'installation, afin de pointer vers la nouvelle distribution.
Il faudra ensuite éxecuter `apt-get update` puis `apt-get dist-upgrade` en
tant que root, redémarrer et éxecuter `tasksel` en tant que root.
C'est pour éviter de télécharger de larges paquets deux fois.

NOTE: si vous voulez la toute dernière version mise à jour du kernel Linux,,
le kernel de Debian est quelquefois en retard, même dans la distribution de test.
Au lieu de ça, vous pouvez considérer d'utiliser [ce dépôt](https://jxself.org/linux-libre/)
qui contient les versions les plus à jour du kernel Linux. Ces kernels sont aussi
déblobbé comme sur Debian, donc vous pouvez vous assurez qu'aucuns blobs binaires ne sont
présents.

Configuration Postfix 
=====================
Si demandé, choisissez *Pas de configuration* (ou peut-être vous voulez choisir 
quelque chose d'autre. C'est votre choix.)

Installer le chargeur d'amorçage GRUB sur le MBR
======================================================

Choisissez 'Oui'. Ça échouera, mais ne vous inquiétez pas. Ensuite sur
le menu principal, choisissez 'Continuez sans un chargeur d'amorçage'.
Vous pouviez aussi choisir 'Non'. Le choix n'a pas d'importance ici.

*N'oubliez pas d'avoir le paquet grub-coreboot installé, même si l'installation de grub sur le MBR
n'a pas d'intérêt sur les systèmes Libreboot, les outils grub sont encore nécessaires pour p.e.
générer la configuration (`grub-mkconfig`)*


Horloge UTC
=========

Répondez 'Oui'.


Démarrer votre système
===================

À ce point vous aurez fini l'installation. Quand vous
arrivez sur votre charge utile GRUB, pressez C pour avoir
la ligne de commande et entrez :
    
    grub> cryptomount -a
    grub> set root='lvm/matrix-rootvol'
    grub> linux /vmlinuz root=/dev/mapper/matrix-rootvol cryptdevice=/dev/mapper/matrix-rootvol:root
    grub> initrd /initrd.img
    grub> boot

ecryptfs
========

Si vous n'avez pas chiffré votre répertoire personnel/maison, alors vous
pouvez ignorer sans souci cette section.

Immédiatement après vous être authentifié, éxecutez :

    $ sudo ecryptfs-unwrap-passphrase

Ça sera nécessaire dans le futur si vous avez un quelconque besoin de récupérer
votre répertoire personne depuis un autre système, donc notez la sortie et gardez
la note dans un endroit tenu secret. Idéalement, vous devriez la mémoriser et ensuite
brûler la note (ou ne même pas l'écrire et quand même la mémoriser).

Modifier grub.cfg (CBFS)
======================

Maintenant vous avez besoin de le configurer de telle façon que le système démarrera
automatiquement, sans avoir à taper tout un tas de commandes.

Modifiez votre grub.cfg (dans le micrologiciel) [en suivant ce
tutoriel](grub_cbfs.md); changez juste l'intérieur du menu par défaut
'Load Operating System' par ceci :

    cryptomount -a
    set root='lvm/matrix-rootvol'
    linux /vmlinux root=/dev/mapper/matrix-rootvol cryptdevice=/dev/mapper/matrix-rootvol:root
    initrd /initrd.img

Sans spécifier un appareil, le paramètre *-a* essaye de dévérouiiller tout les
volumes LUKS détectés. Vous pouvez aussi -u UUID ou -a (appareil bloc).

[Référez vous à ce guide](grub_hardening.md) pour des directives plus poussées sur
l'endurcissement de votre configuration GRUB au niveau sécurité.

Flashez la ROM modifiée en utilisant [ce tutoriel](../install/#flashrom).

Dépannage
===============

Un utilisateur a rapporté des problèmes lors du démarrage et du déchiffrement du disque dans GRUB,
sur son X200 avec station d'accueil attachée. L'erreur *AHCI transfer timed out* a été observée.
La solution de contournement a été d'enlever la station d'accueil.

Une investigation plus poussée a révélé que c'était le lecteur CD/DVD
qui causait problème. L'enlever a résolu l'affaire.

    "sudo wodim -prcap" montre des informations à propos du lecteur:
    
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

Permission est donnée de copier, distribuer et/ou modifier ce document
sous les termes de la Licence de documentation libre GNU version 1.3 ou
quelconque autre versions publiées plus tard par la Free Software Foundation
sans Sections Invariantes,  Textes de Page de Garde, et Textes de Dernière de Couverture.
Une copie de cette license peut être trouvé dans [../fdl-1.3.md](fdl-1.3.md).
