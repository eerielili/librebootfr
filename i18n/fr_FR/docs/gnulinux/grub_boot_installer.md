---
title: Comment préparer et démarrer un installeur USB sur des systèmes Libreboot
x-toc-enable: true
...

Ce guide explique comment préparer une clef USB démarrable pour des systèmes Libreboot, pouvant être utilisée pour installer de nombreuses distributions GNU+Linux.
Pour ce guide vous allez avoir seulement besoin d'une clef USB et de l'utilitaire `dd` (il est installé par défaut dans toute les distributions GNU+Linux).

Pour des informations sur comment installer des distributions GNU+Linux spécifiques, référez-vous à [cette page](index.md).

## Préparer la clef USB dans GNU+Linux
Si vous avez télécharger votre ISO sur un système GNU+Linux existant, voilà comment créer
la clef USB GNU+Linux démarrable:

Connectez la clef USB. Vérifiez `lsblk` pour confirmer son nom de périphérique (p.e., **/dev/sdX**):

    $ lsblk

Pour cet example, assumons que le nom de notre clef est `sdb`. Assurez-vous qu'il n'est pas monté:

    $ sudo umount /dev/sdb

Écrasez le disque, écrivant l'ISO de votre distribution dessus avec `dd`. Par example, si nous installons Trisquel 7.0 64-bit, et qu'il se situe dans notre fichier Téléchargements, c'est la commande que nous voudrons éxecuter;

    $ sudo dd if=~/Téléchargements/trisquel_7.0_amd64.iso of=/dev/sdb bs=8M; sync

C'est tout ! Vous devriez être maintenant capable de démarrer l'installeur depuis votre clef USB (les instructions sur comment faire celà seront données plus tard).

## Préparer la clef USB dans NetBSD
[Cette page](https://wiki.netbsd.org/tutorials/how_to_install_netbsd_from_an_usb_memory_stick/) sur le site de NetBSD montre comment créer une clef USB NetBSD démarrable, depuis NetBSD même. Vous devriez utiliser la méthode `dd` documentée là bas. Ça marchera avec n'importe quelle image ISO GNU+Linux.

## Préparer la clef USB dans FreeBSD
[Cette page](https://www.freebsd.org/doc/handbook/bsdinstall-pre.html) sur le site web de FreeBSD montre comment créer une clef USB démarrable pour installer FreeBSD. Utilisez la méthode `dd` documentée. Ça marchera sur n'importe quelle
image ISO GNU+Linux.


## Préparer la clef USB (sur LibertyBSD ou OpenBSD)

Si vous avez téléchargé votre ISO sur un système LibertyBSD ou OpenBSD, voici
comment créer une clef USB FreeBSD démarrable:

Connectez la clef USB. Regardez la sortie de dmesg:

    $ dmesg | tail

Vérifiez et confirmer de quel USB il s'agit si vous pensez par exemple que c'est sd3:

    $ disklabel sd3

Vérifiez qu'elle n'a pas été montée automatiquement. Sinon, on l'éjecte. Par exemple :

    $ doas umount /dev/sd3i

dmesg vous a dit quel appareil/bloc c'était, donc on peut maintenant écrire par dessus
l'installateur FreeBSD grâce à dd. Par exemple :

    $ doas dd if=freebsd.img of=/dev/rsdXc bs=1M; sync

Vous devriez être maintenant capable de démarrer l'installeur depuis votre clef USB.
Continuez à lire pour apprendre comme faire ceci.

## Installation par le net Debian ou Devuan
Téléchargez le netinstalleur Debian ou Devuan. Vous pouvez télécharger l'ISO Debian 
depuis [la page d'accueil de Debian](https://www.debian.org/), ou l'ISO Devuan depuis
[la page d'accueil Devuan](https://www.devuan.org/).

Deuxièmement, créez une clef USB démarrable en utilisant les commandes de
[#preparer-la-clef-usb-dans-gnulinux](#preparer-la-clef-usb-dans-gnulinux).

Troisièmement, démarrez l'USB et entrez ces commandes dans le terminal GRUB
(pour les intel 64-bit ou AMD):

    grub> set root='usb0'
    grub> linux /install.amd/vmlinuz
    grub> initrd /install.amd/initrd.gz
    grub> boot

Si vous êtes sur un système 32-bit (p.e. quelques ThinkPad X60) alors vous allez 
avoir besoin de ces commandes (c'est aussi vrai pour le 32-bit s'éxecutant sur
les machines 64-bit):

    grub> set root='usb0'
    grub> linux /install.386/vmlinuz
    grub> initrd /install.386/initrd.gz
    grub> boot

NOTE pour les utilisateurs de G41M (32/64bit): Sur la ligne *linux*, spécifiez fb=false
pour démarrer en mode texte ou alors l'installeur n'aura pas d'affichage sur votre écran.

## Démarrer les Images ISOLINUX (méthode automatique)
Démarrez le dans GRUB en utilisant l'option `Parse ISOLINUX config (USB)`. Un nouveau menu devrait apparaître
dans GRUB, montrant les options de démarrage pour cette distribution; c'est un menu GRUB converti depuis le menu ISOLINUX habituel
fourni par cette distribution.

## Démarrer les Images ISOLINUX (méthode manuelle)
Ces instructions là sont génériques. Elle peuvent ou pas être correcte pour votre
distribution. Vous devez les adapter en accordance pour n'importe quelle distribution
GNU+Linux que vous essayez d'installer.

Si les options `ISOLINUX parser` ou `Search for GRUB configuration` ne marche pas, alors
tapez `C` dans GRUB pour accéder à la ligne de commande et ensuite éxecuter la commande
`ls`:

    grub> ls

Récupérez le nom de l'appareil à partir de la sortie ci-dessus (p.e., `usb0`). Voici
un exemple:

    grub> cat (usb0)/isolinux/isolinux.cfg

La sortie de cette commande sera soit les entrées de menus ISOLINUX pour cette ISO,
ou des liens vers d'autres fichiers `.cfg` (p.e. **/isolinux/foo.cfg**). Par exemple, si le fichier
trouvé était **foo.cfg**, vous voudrez éxecuter cette commande:

    grub> cat (usb0)/isolinux/foo.cfg

Et ainsi de suite, jusqu'à que vous trouvez les bonnes entrées de menu pour ISOLINUX.

Pour les distributions basées Debian (p.e. Trisquel, Devuan), il y a typiquement des entrées de
menu listées dans **/isolinux/txt.cfg** ou **/isolinux/gtk.cfg**. Pour les images ISO à double 
architecture (i686 et x86\_64), il devrait avoir des répertoires de fichiers séparés pour chacunes.
Contentez-vous de chercher à travers l'image jusqu'à ce que vous trouvez le bon fichier de configuration ISOLINUX.

**NOTE: L'ISO de Debian 8.6 liste seulement des options de démarrage 32-bit dans txt.cfg. C'est important à retenir si vous
voulez du démarrage 64-bit sur votre système. Les versions de Devuan basées sur Debian 8.x peuvent avoir le même soucis.**

Maintenant, regardez l'entrée de menu ISOLINUX; elle devrait ressembler à ça:

    kernel /path/to/kernel append PARAMETERS initrd=/path/to/initrd ...

GRUB marche de façon similaire; voici quelques examples de commandes GRUB:

    grub> set root='usb0'
    grub> linux /chemin/vers/kernel PARAMÈTRES PEUTETRE_PLUS_DE_PARAMETRES
    grub> initrd /chemin/vers/initrd
    grub> boot

Note: `usb0` peut être incorrect. Jetez un coup d'oeil à la sortie de la commande `ls` (dans GRUB), pour voir une liste
de périphériques USB/partitions. Bien sûr, ça variera entre distributions. Si vous avez fait tout ça correctement, alors
ça devrait maintenant démarrer votre clef USB de la manière que vous avez spécifié.

## Dépannage


GRUB works similarly; here are some example GRUB commands:

    grub> set root='usb0'
    grub> linux /path/to/kernel PARAMETERS MAYBE\_MORE\_PARAMETERS
    grub> initrd /path/to/initrd
    grub> boot

Note: `usb0` may be incorrect. Check the output of the `ls` command (in GRUB), to see a list of USB devices/partitions. Of course, this will vary from distro to distro. If you did all of that correctly, then it should now be booting your USB drive in the way that you specified.

## Troubleshooting
Most of these issues occur when using Libreboot with Coreboot's `text-mode`, instead of the Coreboot framebuffer. This mode is useful for booting payloads, like `MemTest86+`, which expect `text-mode`, but for GNU+Linux distributions, it can be problematic when they are trying to switch to a framebuffer, because it doesn't exist.

In most cases, you should use the **vesafb** ROM images. An example filename would be **libreboot\_ukdvorak\_vesafb.rom**.

### Parabola Won't Boot in Text-Mode
Use one of the ROM images with `vesafb` in the filename (uses Coreboot framebuffer, instead of `text-mode`).

### debian-installer Graphical Corruption in Text-Mode (Debian and Devuan)
When using the ROM images that use Coreboot's `text mode`, instead of the Coreboot framebuffer, booting the Debian or Devuan net installer results in graphical corruption, because it is trying to switch to a framebuffer, which doesn't exist. Use that kernel parameter on the `linux` line, when booting it:

    vga=normal fb=false

This forces debian-installer to start in `text-mode`, instead of trying to switch to a framebuffer.

If selecting `text-mode` from a GRUB menu created using the ISOLINUX parser, you can press `E` on the menu entry to add this. Or, if you are booting manually (from GRUB terminal), then just add the parameters.

This workaround was found on the [Debian site](https://www.debian.org/releases/stable/i386/ch05s04.html). It should also work for Devuan, and any other `apt-get` distro that provides the debian-installer (i.e., text-mode) net install method.

Copyright © 2014, 2015, 2016 Leah Rowe <info@minifree.org>

Copyright © 2016 Scott Bonds <scott@ggr.com>

Copyright © 2017 Elijah Smith <esmith1412@posteo.net>

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License Version 1.3 or any later version published by the Free Software Foundation with no Invariant Sections, no Front Cover Texts, and no Back Cover Texts. A copy of this license is found in [../fdl-1.3.md](../fdl-1.3.md)
