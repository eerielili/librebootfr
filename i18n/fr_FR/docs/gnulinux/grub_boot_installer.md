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

## Booting ISOLINUX Images (Manual Method)
These are generic instructions. They may or may not be correct for your distribution. You must adapt them appropriately, for whatever GNU+Linux distribution it is that you are trying to install.

If the `ISOLINUX parser` or `Search for GRUB configuration` options won't work, then press `C` in GRUB to access the command line, then run the `ls` command:

    grub> ls

Get the device name from the above output (e.g., `usb0`). Here's an example:

    grub> cat (usb0)/isolinux/isolinux.cfg

Either the output of this command will be the ISOLINUX menuentries for that ISO, or link to other `.cfg` files (e.g, **/isolinux/foo.cfg**). For example, if the file found were **foo.cfg**, you would use this command:

    grub> cat (usb0)/isolinux/foo.cfg

And so on, until you find the correct menuentries for ISOLINUX.

For Debian-based distros (e.g., Trisquel, Devuan), there are typically menuentries listed in **/isolinux/txt.cfg** or **/isolinux/gtk.cfg**. For dual-architecture ISO images (i686 and x86\_64), there may be separate files directories for each architecture.  Just keep searching through the image, until you find the correct ISOLINUX configuration file.

**NOTE: Debian 8.6 ISO only lists 32-bit boot options in txt.cfg. This is important, if you want 64-bit booting on your system. Devuan versions based on Debian 8.x may also have the same issue.**

Now, look at the ISOLINUX menuentry; it'll look like this:

    kernel /path/to/kernel append PARAMETERS initrd=/path/to/initrd ...

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
